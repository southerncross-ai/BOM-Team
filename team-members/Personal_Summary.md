# CRAFT Project – Personal Summary

## Dylan
### My contributions

My work focused on understanding the CRAFT architecture, preparing the ACCESS-SY data workflow, and developing a small notebook to test the model pipeline. I contributed to reviewing CRAFT and NVIDIA ATLAS and documenting the hourly U/V wind data, preprocessing decisions and tensor interfaces. I also prepared a technical documentation draft covering reproducibility requirements and outstanding work.

The local synthetic-data smoke test successfully checked data loading, encoding, latent evolution and decoding over six hourly forecast steps. This confirmed that the basic data flow and forward/backward pass worked at small scale.

### What I learned

I gained experience with NetCDF, xarray and PyTorch, particularly in preparing wind data and managing spatiotemporal tensor shapes. I learned why chronological validation and training-only normalization are necessary to avoid data leakage. The project also helped me understand the difference between getting a model pipeline running and demonstrating useful forecasting performance.

### Remaining work

The prototype uses a simplified ConvGRU-style temporal component. Further work includes implementing the planned Transformer, validating on genuine ACCESS-SY data, correcting the split and normalization procedure, and comparing forecasts against a persistence baseline. The final environment, configurations and results also need to be recorded for reproducibility.

## Kecheng
### My contributions

I built the CRAFT model and data pipeline as two parts. `model.ipynb` implements the full architecture end to end: a CNN encoder `E` compressing each wind frame into latent tokens, a latent autoregressive Transformer `F` with factorized axial attention (temporal, row, column), and a mirror decoder `D`, trained as one graph with scheduled sampling and pure-latent rollout. `data_process.ipynb` turns the raw ACCESS-SY analysis fields into a cropped 744 x 892 grid, interpolates 10-minute intermediates and streams them to parquet. For the main experiment I added `server_data_process.ipynb` and `server_model.ipynb` for the native hourly server data: sharded parquet with a manifest, gap-aware windowing, lazy LRU loading, a chronological 70/15/15 split and train-only normalization cached to a sidecar.

Most of the later work included reducing computational cost and bug fixing. The recorded run reached best validation MSE 0.7315 and beat the hourly-linear baseline by +80.7% (u) and +76.1% (v) at 12 h lead, with reconstruction RMSE 2.05/3.15 m/s. I also wrote the experiments runbook and updated the contributor docs.

### What I learned

I learned how a latent nowcasting model behaves in practice: what the encode/decode bottleneck discards, why rollouts accumulate error and inflate variance at longer leads, and why axial factorization is what makes spatial compression pay off. Most performance problems turned out to be memory behaviour rather than compute, and a bug-free pipeline is still a long way from a meaningful forecast.

### Remaining work

The notebooks train on u/v wind only while the paper defines five surface channels, and the processed record held ~5 months of hourly data rather than 12. Short-range skill is still weak (persistence beats the model at lead 1, -39.9% for v-wind), and the recorded run used 3 epochs at reduced width and depth, so it validates the pipeline, not model capacity. The planned ablations (compression factor, latent width, temporal vs axial attention, sampling schedule, reconstruction loss, full-resolution baseline) and full-archive training on Gadi remain.

## Junling
### My contributions

My contributions focused on preparing and exploring weather data, reproducing the YSSY processing and LightGBM baseline workflow, and investigating temporal interpolation for ACCESS-SY wind fields. I implemented and visualised alternative interpolation methods, including semi-Lagrangian advection, and reviewed the CRAFT paper to identify questions about model design and evaluation.

### What I learned

I learned how to process and align meteorological datasets, evaluate forecasting baselines, and distinguish smooth-looking interpolated fields from physically realistic and useful predictions. I also developed a better understanding of model assumptions, computational costs, and error accumulation in multi-step forecasting.

### Remaining work

Further work includes quantitatively comparing interpolation methods and testing whether they improve downstream forecasting performance.

## Adam
### My contributions

I was the project manager. I ran the team’s Agile process: I broke the work into product backlog items (PBIs), set milestones, tracked progress against them, and kept the five of us coordinated across the data, model and paper workstreams so parallel work didn’t stall or duplicate. This meant regularly reprioritising the backlog as the project changed, such as when we shifted from local synthetic tests to the native hourly server data, and flagging blockers early so we stayed on schedule.

I also wrote extensive documentation for the CRAFT repository. This included:

the project overview explaining CRAFT’s frame-based design (masked multi-channel weather frames, a CNN for spatial structure, a Transformer for temporal evolution)
the student contributor table
the shared notes and ideas scratchpad
the repository layout covering Paper/, CRAFT/, Docs/ and the gitignored data/ directory

The goal was a single entry point, so contributors could find the paper draft, research guide and experiments runbook without piecing the structure together themselves.

I also reviewed the CRAFT paper in detail. I focused on the architecture (encoder E, latent autoregressive Transformer F, decoder D), the data and evaluation methodology, and how closely the written design matched what we had implemented. I checked the ACCESS-SY data description against the processing pipeline, including the u/v wind channels, the cropped 744 x 892 grid, the chronological 70/15/15 split and train-only normalization.

### What I learned

I learned how to run Agile on a research project, where the scope is less predictable than in typical software work. Experiments take longer than planned, results change priorities, and PBIs and milestones need regular adjustment instead of being fixed once. I also saw how much clear documentation matters when several people work in parallel notebooks.

Reviewing the paper showed me where a design document and a working prototype drift apart. The paper defines five surface channels and a 12-month record, while current runs use u/v wind only and about 5 months of hourly data. I also gained a better understanding of latent nowcasting, error accumulation in autoregressive rollouts, and why persistence and linear baselines are needed to demonstrate real forecasting skill.


