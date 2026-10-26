# CRAFT Data Processing, Channel Inventory and Data Access

## 1. Purpose, scope and data source

This document records how CRAFT data is processed, which variables (channels) it contains, and how team members get access to it.

- **Data Source:** the ACCESS-SY wind dataset, **a high-resolution regional weather forecast model** operated by BoM, the Sydney-domain configuration of the Bureau of Meteorology's convection-allowing ACCESS-C weather prediction system, on a grid of about 1.5 km. It is supplied to the team by the project stakeholder and held on the project JupyterHub.
- **Size and coverage:** one NetCDF file of about 139 GB, roughly 26,000 hourly states from September 2020 to May 2024. The data was extracted on Gadi (Australia’s national research supercomputer, run by the National Computational Infrastructure (NCI)) into 4 years of hourly wind maps.
- **Grid:** each state is 744 x 892 cells, covering the ACCESS-SY domain around Sydney.
- **Privacy:** the data and anything derived from it are private project data.

## 2. Data processing workflow

data preprocessing turns the server NetCDF file into sharded parquet, then turns that parquet into normalised windows for the model.

**Part A: NetCDF to sharded parquet**

1. **Open and check the source.** Open the netCDF dataset and Assert the dimensions are time, lat and lon, the two wind variables exist, latitude is descending and longitude is ascending.
2. **Select the period and crop.** By default the last 365 days of the record (or chosen start and end dates), over the full Sydney domain (latitude -38.0 to -27.97, longitude 144.0 to 156.03). The grid must come out as 744 x 892.
3. **Find time gaps.** The source keeps genuine missing hours. The notebook splits the record into contiguous one-hour runs and keeps only runs of at least 19 hours (K + H\_STEPS). Nothing is interpolated or windowed across a gap.
4. **Write sharded parquet and a manifest.** Long format with columns time, lat, lon, `uwnd10m`, `vwnd10m` (float32, snappy). One row group is one hourly frame, 512 frames per shard file. `manifest.json` records each frame's location, the contiguous runs, the times and any frames containing NaN. Output folder: `an_wind_hourly_12mo` (about 35 to 50 GiB expected for 12 months).
5. **Verify.** Check schema and row counts, and compare 8 random frames bit-for-bit with the source file.

**Part B: parquet to model input**

6. **Build windows.** Each window is K = 7 context hours plus H\_STEPS = 12 target hours, and must sit inside one contiguous run. Windows touching a NaN frame are dropped.
7. **Split by time.** Chronological 70% train, 15% validation, 15% test over the valid windows.
8. **Normalise.** Per-channel mean and standard deviation come from frames used by training windows only, streamed in float64 and saved. Each frame is then scaled as (frame - mean) / std when it is loaded.
9. **Load.** Frames are read from parquet on demand with a small cache, so the full record never sits in memory. Batch size is 1 and each epoch uses a subsample of windows (200 train, 100 validation).

The data consist of 5 channels : `uwnd10m`, `vwnd10m`, `wndgust10m`, `temp_scrn`, `sfc_pres`

- The team uses hourly states.&#32;
- Statistics are computed in float64, which avoids the float32 error that gave a wrong temperature mean (119.8 instead of about 289) earlier.
- Data is stored sharded because a year of hourly frames is too large for one file.

## 3. After data processing : model overview

The processed windows feed CRAFT, which forecasts inside a compressed latent space.

1. **Encoder (CNN)** compresses each frame, for example 2 x 744 x 892 into a latent grid of 64 x 186 x 223 (spatial factor S = 4, latent channels D = 64).
2. **Tokens:** each latent grid cell is one token for the transformer. The CNN does not output tokens directly.
3. **Temporal transformer** reads the last K latent frames and predicts the next one.
4. **Rollout:** the prediction is appended and the step repeats in latent space.
5. **Decoder** turns predicted latents back into wind fields when a physical output is needed.

All three parts train together as one network. The final notebook uses one-hour steps, 7 context hours and a 12-hour forecast, which matches the team poster. The lead's earlier email planned about six hours.

## 4. Variable and channel inventory

The wind file named in the lead's email holds two channels (10 m wind U and V); the CRAFT paper plans five, and rain was dropped. Rain is therefore not a channel. The team poster says the final model is trained on U and V wind only, so the other three planned channels are future work.

| Channel | Meaning | Unit |
| --- | --- | --- |
| `de_uwnd10m` (`uwnd10m` in the paper) | Eastward wind at 10 m | m/s |
| `de_vwnd10m` (`vwnd10m` in the paper) | Northward wind at 10 m | m/s |
| `wndgust10m` | Wind gust at 10 m | m/s |
| `temp_scrn` | Screen-level temperature | K |
| `sfc_pres` | Surface pressure | Pa |
| `accum_rain` | Accumulated rain | to confirm |

**Grid and time:** 744 x 892 cells, hourly, September 2020 to May 2024.

## 5. Data access instructions

Team members reach the data through the project JupyterHub at https://jupyterhub.southerncross.ai.

1. Log in to the JupyterHub and open the notebook environment.
2. Open the dataset which is read-only, so it cannot be changed by accident.
3. Load : `ds = xr.open_dataset(path, chunks="auto")`, then select with `isel` or `sel` and call `.load()` only on small slices.
4. Start from the sample notebook.
5. Use CPUs for data work. GPUs are for the later training stage.
6. Write test datasets and intermediate files to the shared folder.&#32;

**Rules:** treat the dataset and all derived data as private. Keep them inside the project environment and the team. Do not upload copies to public repositories, external storage, or anywhere outside the team.
