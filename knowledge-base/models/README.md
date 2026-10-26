# Models 🧠


## Current CRAFT training and evaluation

The maintained model workflow is in the [CRAFT repository](https://github.com/southerncross-ai/Swift).
Code under BOM-Team's personal directories is reference and historical material.

| Order | Resource | Purpose |
|---|---|---|
| 1 | [Setup guide](https://github.com/southerncross-ai/Swift/blob/main/SETUP.md) | Prepare the project environment |
| 2 | [Server data notebook](https://github.com/southerncross-ai/Swift/blob/main/CRAFT/server_data_process.ipynb) | Create and verify Parquet shards and manifest |
| 3 | [Server model notebook](https://github.com/southerncross-ai/Swift/blob/main/CRAFT/server_model.ipynb) | Train, evaluate against baselines, visualize, and save artifacts |
| Reference | [Experiment runbook](https://github.com/southerncross-ai/Swift/blob/main/Docs/model-experiments-runbook.md) | Configuration, checkpoint paths, recorded results, and reproduction limits |

Set the kernel working directory to `CRAFT/`. After preprocessing verification
passes, review the model notebook's GLOBAL PARAMETERS, then run it from top to
bottom. Its `RUN_DIR`, channels, grid, `K`, and `H_STEPS` must match the manifest.
Evaluation is included in this notebook; there is no separate required evaluation script.

Artifacts are `ckpt_server/craft_server_best.pt` and
`ckpt_server/craft_server_stats.json`. Preserve them together and reuse the matching
normalization statistics for inference. Read the runbook before changing memory,
precision, or loading settings.

For data context, see the [ACCESS-SY index](../data/README.md). For preliminary-result
limitations, split-boundary checks, and next steps, see the
[project handover status](../../README.md#current-project-status-and-handover).
