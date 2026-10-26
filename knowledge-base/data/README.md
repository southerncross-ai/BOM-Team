# 📂 Data Environment & Rules

Welcome to the **BOM Nowcasting** data environment. This directory contains the documentation for accessing and working with our shared project datasets. 

Because we are dealing with massive amounts of data—specifically a 60 TB corpus—it is critical that you understand how our storage architecture works to avoid crashing the system or breaking your personal workspace.

## 💾 Storage Limits & Quotas

Each student is allocated a strict **50 GB personal storage quota** for their notebook environment. This space is intended solely for your code, small test files, environment configurations, and limited output data.

**🛑 Rule #1: Never copy shared datasets to your personal directory.**

Attempting to copy even a small fraction of our main datasets (like a handful of 2 GB Parquet files) will instantly exhaust your 50 GB quota, freeze your notebook, and break your environment. Always read the data directly from the shared, read-only folders into memory.

---

## ⚠️ CRITICAL WARNING: Data Persistence & GitHub

While you can process data and save the outputs to your 50 GB personal allocation, **there is no guarantee that your saved data will be there from week to week.** This is a dynamic, evolving environment. Storage may be wiped, instances may be reset, or your quota may need to be cleared to fix an issue. 

**You must commit your code to the GitHub repository regularly.** In data engineering, your cleaned data is completely disposable, but the *code* that cleans it is your most valuable asset. If your processed data is wiped, you can simply run your pipeline again. If you lose your pipeline code, you lose weeks of work. **Commit your pipelines to GitHub!**

---

## 📚 Documentation Index

Please read through the following documents to understand how to interact with the data safely and efficiently:

| File | Description |
| :--- | :--- |
| [Fineweb.md](Fineweb.md) | Details on the 60 TB Fineweb corpus, the read-only mount, and the Parquet file structure. |
| [LoadingData.md](LoadingData.md) | Code examples for loading data efficiently on CPU (Pandas/PyArrow) vs. GPU (cuDF). |
| [L4Gpu.md](../compute-infrastructure/L4Gpu.md) | Crucial VRAM management rules and OOM (Out of Memory) prevention for the L4 GPUs. |
| [BestPractices.md](BestPractices.md) | The "Subset First" rule, memory management, and how to transition code into background jobs. |
| [Publishing.md](Publishing.md) | Hugging Face, Kaggle & Data Cards |


## Current ACCESS-SY server workflow

The Fineweb corpus described above is separate from the ACCESS-SY weather dataset
used by CRAFT. Retain the storage rules above and use the following resources for
the current weather-data workflow:

| Resource | Purpose |
|---|---|
| [ACCESS-SY data processing, channel inventory and access](<../../team-members/olivia-maftukhaturrizqoh/notes/CRAFT Data Processing, Channel Inventory and Data Access.md>) | Dataset description, preprocessing decisions, variables, and private data access |
| [CRAFT setup guide](https://github.com/southerncross-ai/Swift/blob/main/SETUP.md) | Python environment and notebook setup |
| [Server data notebook](https://github.com/southerncross-ai/Swift/blob/main/CRAFT/server_data_process.ipynb) | Native hourly NetCDF to Parquet shards and manifest, including output verification |
| [Model and experiments runbook](https://github.com/southerncross-ai/Swift/blob/main/Docs/model-experiments-runbook.md) | Processing parameters, paths, configuration, and reproduction steps |
| [Gadi archive notes](https://github.com/southerncross-ai/Swift/blob/main/gadi.md) | Source archive, extraction history, and known time gaps |
| [Training and evaluation entry points](../models/README.md) | Next step after preprocessing |

Run `CRAFT/server_data_process.ipynb` from top to bottom with the kernel working
directory set to `CRAFT/` in the Swift repository. Review its source path, time
selection, output directory, and `OVERWRITE` setting first. Proceed to training only
after the final verification cell passes.

The output is `an_wind_hourly_12mo/` containing shards and `manifest.json`; the name
does not establish actual coverage. The main workflow uses hourly U/V wind and
preserves source gaps. Keep source and derived data in approved private project
storage. See the runbook for resource requirements before selecting an output location.
