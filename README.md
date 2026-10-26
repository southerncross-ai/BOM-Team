# BOM Nowcasting 🌦️

Welcome. This repository is the central hub for the BOM Nowcasting team. We use this space to organize the project, manage documentation, and track progress.

---

## Project Vision

BOM Nowcasting is focused on building practical short-term forecasting workflows end to end.

The team's work moves through a practical pipeline:
* **Collect & Organize:** Work with observational, historical, and derived weather data.
* **Prepare & Validate:** Clean inputs, align time windows, and validate data quality.
* **Model & Evaluate:** Develop nowcasting models and assess forecast accuracy against operational needs.

The goal is to build reliable nowcasting workflows while understanding how data quality, infrastructure, and modeling choices affect forecast performance.

---

## 🛑 The "Stay in GitHub" Rule

**Keep all project work inside GitHub.** GitHub already provides everything we need for documentation, planning, and task tracking. 

> **⛔ Important:** You may receive suggestions from tutors, lecturers, or others to use different third-party tools (like Notion, Jira, or Trello) for planning or documentation. **Those are only suggestions.** For this project, keep *everything* inside this repository and the GitHub platform so our work stays organized, consolidated, and easy to review.

---

## 🚦 Start Here

Use these links first:

1. `roadmap/ProjectGoal.md`
2. `knowledge-base/compute-infrastructure/Wireguard.md`
3. `team-members/README.md`
4. `management/weekly-reports/README.md`
5. `team-members/README.md` 📓
6. `management/weekly-todos/README.md` ✅

---

## 🚀 Where to Start

If you are a new team member, please read these documents first to get oriented:

1. `roadmap/ProjectGoal.md`
2. `knowledge-base/compute-infrastructure/Wireguard.md`
3. `team-members/README.md`
4. `management/weekly-reports/README.md`
5. `team-members/README.md` 📓
6. `management/weekly-todos/README.md` ✅

---

## 🗂️ Repository Structure

This repository is strictly for documentation and project management (no code!). Here is how our knowledge base is organized:

| Folder | Purpose |
| :--- | :--- |
| **`knowledge-base/`** | Combined compute infrastructure, data, models, learning resources, Q&A, papers, and repository/platform references. |
| **`management/`** 🚦 | Weekly reports, weekly TODOs, and team coordination docs. |
| **`roadmap/`** | Project goals, planning docs, and semester overviews. |
| **`team-members/`** | Combined member profiles and notebook progress tracking. |

---

## 📊 Project Tracking & Usage

**📦 What This Repository Is For**
* Project documentation and architecture rules
* Planning and task tracking
* Combined weekly meeting notes and progress reports
* Team organization

**📋 The GitHub Project Board**
We use a GitHub Project Kanban board called **BOM Nowcasting Team** to manage tasks and track progress. All tasks, issues, and progress updates should be recorded on this board so the work remains visible and organized for everyone.

---

## 📚 Documentation Guidelines

All documentation here should be written so it is easy to read directly in GitHub's web interface. When adding or editing files, please follow these rules:

**✍️ Format**
* Use standard Markdown (`.md`) files.
* Write documents so they are clear and highly scannable.
* Use headers, bullet points, and lists to break up walls of text.
* Keep files structured and logically organized within their folders.

**✂️ Keep It Short**
* Try not to let documentation pages grow too massive.
* Keep documents short, punchy, and concise.
* If a document becomes too long, split it into a new, separate file.
* Each document should ideally focus on **one main topic**. 

*This makes the documentation easier to read, maintain, and navigate for the whole team! 🙂*


---

## Current Project Status and Handover

**Status reviewed: 26 October 2026.** This section supplements the original project
rules and onboarding information above. Use this section and the linked CRAFT
runbook for the current implementation and experiment workflow.

### Repository responsibilities

| Repository | Current responsibility |
|---|---|
| [BOM-Team](https://github.com/southerncross-ai/BOM-Team) | Team coordination, meeting reports, contribution summaries, reference material, and handover navigation |
| [CRAFT (repository named Swift)](https://github.com/southerncross-ai/Swift) | Model implementation, server experiment notebooks, setup instructions, paper, and experiment runbook |

The notebooks and scripts under `team-members/` in BOM-Team are reference,
learning, and historical exploration material. The operational experiment runs in
the CRAFT repository. This clarifies the earlier repository-structure description:
BOM-Team contains reference code, while CRAFT maintains the current model workflow.

### Current implementation

CRAFT implements a CNN encoder, a latent temporal Transformer, and a decoder in
notebooks. The main server experiment uses native hourly ACCESS-SY U/V wind fields
on a 744 × 892 grid, seven context frames, and twelve hourly forecast steps.

The server data pipeline, training and evaluation notebook, saved experiment
outputs, and reproduction guide are committed to the CRAFT repository. The recorded
run used 2,999 hourly frames, a reduced model, three training epochs, and limited
window budgets. It is an initial experiment; full-scale training and broader
validation remain outstanding. The output directory name `an_wind_hourly_12mo`
does not establish actual data coverage.

### Operational server notebooks

Paths below are relative to the CRAFT repository root.

| Order | Notebook | Purpose |
|---|---|---|
| 1 | [CRAFT/server_data_process.ipynb](https://github.com/southerncross-ai/Swift/blob/main/CRAFT/server_data_process.ipynb) | Validate and select the source data, preserve time gaps, export Parquet shards and a manifest, and verify the output |
| 2 | [CRAFT/server_model.ipynb](https://github.com/southerncross-ai/Swift/blob/main/CRAFT/server_model.ipynb) | Construct windows, normalize inputs, train, evaluate against baselines, visualize forecasts, and save artifacts |

Read the [CRAFT setup guide](https://github.com/southerncross-ai/Swift/blob/main/SETUP.md)
and [model and experiments runbook](https://github.com/southerncross-ai/Swift/blob/main/Docs/model-experiments-runbook.md)
before running. Set the notebook kernel working directory to the repository's
`CRAFT/` directory. Run the data notebook from top to bottom and complete its
verification checks before running the model notebook. Evaluation is included in
the model notebook; there is no separate required evaluation script.

The source is `/home/jovyan/BOM_Data/access_sy_wind_de_uwnd10m_de_vwnd10m.nc` in the
private project environment. Outputs are `CRAFT/an_wind_hourly_12mo/` and
`CRAFT/ckpt_server/` when the notebooks run from `CRAFT/`. Source and derived data,
checkpoints, and normalization statistics must remain in approved private project
storage. Preserve the checkpoint with its matching normalization sidecar.

### Data, preprocessing, and decisions

| Document | Contents |
|---|---|
| [ACCESS-SY data processing, channel inventory and access](<team-members/olivia-maftukhaturrizqoh/notes/CRAFT Data Processing, Channel Inventory and Data Access.md>) | Source, variables, grid, preprocessing, normalization, and data access |
| [CRAFT runbook — Docs/model-experiments-runbook.md](https://github.com/southerncross-ai/Swift/blob/main/Docs/model-experiments-runbook.md) | Server and local workflows, processing parameters, configurations, artifact paths, and reproduction steps |
| [CRAFT Gadi notes — gadi.md](https://github.com/southerncross-ai/Swift/blob/main/gadi.md) | Archive location, extraction history, source gaps, and unresolved archive questions |
| [Week 14 report](management/weekly-reports/Week14Report.md) | Interpolation experiments and the decision to use native hourly states for the main workflow |

The main server workflow uses `de_uwnd10m` and `de_vwnd10m`, renamed to `uwnd10m`
and `vwnd10m`. It preserves genuine time gaps and builds windows within contiguous
hourly runs. Synthetic ten-minute intermediate states belong to the separate local
exploration workflow. The paper's five-channel source state is broader than the
current two-channel server experiment.

### Experiments and outstanding work

| Document | Contents |
|---|---|
| [CRAFT runbook, sections 5–7](https://github.com/southerncross-ai/Swift/blob/main/Docs/model-experiments-runbook.md) | Recorded server results, reproduction steps, and reproducibility limits; section 2 covers runtime workarounds |
| [Server model notebook](https://github.com/southerncross-ai/Swift/blob/main/CRAFT/server_model.ipynb) | Saved metrics, plots, implementation details, and training/evaluation comments |
| [Team contribution summaries](team-members/Personal_Summary.md) | Contributions, limitations, and remaining research work |
| [CRAFT documentation map](https://github.com/southerncross-ai/Swift/blob/main/DOCUMENTATION_SUMMARY.md) | Documentation responsibilities and known documentation cleanup |
| [Week 20 report](management/weekly-reports/Week20Report.md) | Delivery requirements and consolidation tasks |

The reference server run is recorded at CRAFT commit `c3d01db` (9 October 2026).
The runbook and workspace documentation were updated in `97df952`, followed by
contributor-documentation corrections in `2f17aef` (20 October 2026).

Current limitations and follow-up checks:

- Training and evaluation used reduced budgets; short-lead performance is mixed.
- Audit sliding-window overlap at train/validation/test boundaries before treating
  the recorded evaluation as an independent final benchmark.
- Review normalization-cache validity when changing the source data or split.
- Record a fresh-environment reproduction and resolve the missing committed
  dependency lockfile: `uv.lock` is currently ignored.
- Checkpoints and matching normalization sidecars remain on the training machine;
  preserve their locations and access arrangements for handover.
- Planned ablations and full-archive training remain incomplete. There is no
  automated test suite.

### Logical next steps

1. Verify split boundaries and normalization-cache validity, then rerun evaluation.
2. Reproduce the server workflow in a fresh environment and record dependencies,
   hardware, configuration, and persistent artifact locations.
3. Expand training and evaluation, checking both persistence and hourly-linear
   baselines, especially at short leads.
4. Run the planned compression, latent-width, attention, sampling, reconstruction,
   and full-resolution baseline ablations.
5. Keep the paper, notebook headers, data documentation, and runbook aligned with
   the actual experiment. Maintain operational details in CRAFT and coordination
   records in BOM-Team.
