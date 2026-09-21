# Week 16 Report 🗂️

**Week:** Week 16  
**Date:** 2026-09-21  
**Attendees:** Kecheng, Junling, Olivia, Adam, Dylan  
**Stakeholder:** Matt (unavailable this week)

---

## Agenda

1. Review the model implementation completed last week.
2. Plan the first complete server run using the ACCESS-SY data pipeline.
3. Define the validation work that must be completed within this Sprint.
4. Assign ownership for server execution, validation, and documentation.
5. Review stakeholder communication and follow-up.

## Discussion

> The team met internally this week because the stakeholder was unavailable.  Work will continue against the agreed Sprint goal.
>
> The model implementation was completed last week. The immediate task is now to run the full pipeline on the project server with the ACCESS-SY data, confirm that the environment and dependencies are correct, and resolve any data-loading, tensor-shape, memory, or runtime issues.
>
> Validation must be completed within the current Sprint. This includes preparing a fixed validation split, selecting baseline and model metrics, running the validation process, and recording results in a form that can be reproduced. The team will first confirm that the model runs reliably on a small server-side dataset before increasing the data volume.
>
> Documentation is part of the Sprint deliverable. A first draft should be prepared while the server and validation work is in progress rather than left until the final week. The final documentation package must be ready for the Sprint review in two weeks and should explain the data pipeline, model structure, server setup, validation method, results, known limitations, and reproduction steps.

## Sprint Goal

> Run the completed CRAFT model implementation on the project server, validate it using a reproducible ACCESS-SY validation workflow, and prepare the technical documentation required for the Sprint review.

## Decisions

- Continue implementation and validation work while waiting for stakeholder feedback.
- Use the completed model implementation as the baseline for server integration.
- Run a small server-side test first, then increase the data volume after the pipeline is stable.
- Complete validation within the current Sprint.
- Treat documentation as a Sprint deliverable and update it alongside the technical work.
- Keep all ACCESS-SY source and derived data inside the private project environment.

## Action Items

| Action | Owner | Due |
|--------|-------|-----|
| Confirm the server environment, Python dependencies, GPU access, and writable output paths | Kecheng | Week 16 |
| Prepare the server-side ACCESS-SY data loader and a manageable test dataset | Kecheng, Dylan | Week 16 |
| Run the completed model on the server and verify the full forward and backward passes | Junling, Olivia | Week 17 |
| Debug data-loading, tensor-shape, memory, and runtime issues found during the server run | Junling, Kecheng, Olivia | Week 17 |
| Run validation and record model and baseline results | Dylan, Junling, Olivia | Week 17 |
| Record experiment settings, commands, dependencies, tensor shapes, and output locations | Adam | Ongoing |
| Draft the data, model, server setup, validation, and reproduction documentation | Adam, Dylan | Week 17 |
| Review the documentation against the Sprint checklist | All team members | Week 17 |
| Prepare the final Sprint demonstration and documentation package | All team members | Week 18 |

## Progress Summary

> The model implementation is complete. The team is moving into server integration and validation, with documentation running in parallel. Stakeholder feedback is still pending, but it does not currently prevent the team from running the agreed technical work.

## Completed Last Week

- Completed the initial model implementation.
- Connected the encoder, latent representation, temporal evolution, and decoder components.
- Prepared the codebase for server-side execution and validation.

## In Progress

- Configuring and testing the model in the server environment.
- Connecting the ACCESS-SY data loader to the completed model.
- Running the first server-side forward and backward passes.
- Defining the validation split, baseline, metrics, and result format.
- Writing the first draft of the Sprint documentation.
- Following up with the stakeholder by email.

## Blockers and Risks

> The stakeholder was unavailable this week, and the team has not yet received a response to its email. This is a communication risk rather than a technical blocker. The team will continue with the agreed Sprint goal and record any assumptions that may need stakeholder confirmation.
>
> The model has not yet been fully verified on the server. GPU availability, memory use, dependency versions, data-loading speed, and output permissions need to be checked during the first run.

## Sprint Checklist

### Server execution

- [ ] Confirm the server environment and dependency versions.
- [ ] Confirm GPU availability and device selection.
- [ ] Load a private ACCESS-SY test dataset from the project environment.
- [ ] Complete a forward pass on the server.
- [ ] Complete a backward pass and optimizer step on the server.
- [ ] Record runtime, memory use, and any server-specific fixes.

### Validation

- [ ] Define a chronological train/validation split with no time leakage.
- [ ] Define the persistence baseline.
- [ ] Confirm validation metrics and units.
- [ ] Run the model and baseline on the same validation period.
- [ ] Save numerical results and example forecast outputs.
- [ ] Record limitations and unresolved validation issues.

### Documentation

- [ ] Document the ACCESS-SY input variables, shapes, preprocessing, and normalization.
- [ ] Document the model architecture and tensor interfaces.
- [ ] Document the server environment, dependencies, and run commands.
- [ ] Document the training and validation configuration.
- [ ] Document the baseline, metrics, results, and interpretation.
- [ ] Add reproduction steps and expected output locations.
- [ ] Add known issues, assumptions, and stakeholder questions.
- [ ] Review all links, commands, paths, figures, and tables before delivery.
- [ ] Prepare a final document set for the Sprint review in two weeks.

## Plan for Next Week

- Finish any remaining server integration fixes.
- Run the agreed validation workflow on the server.
- Compare the model results with the persistence baseline.
- Review failed or unstable cases and record their causes.
- Complete the main documentation sections and add validation results.
- Follow up again if the stakeholder has not responded.
- Prepare the model run and documentation for the final Sprint review.

## Next Meeting

**Date:** 2026-09-28
**Focus:** Server results, validation progress, documentation review, and stakeholder response  
🙂

## Sprint Review

**Target date:** 2026-09-21  
**Expected deliverables:** Server-run model, validation results, baseline comparison, reproducible run instructions, and completed technical documentation.
