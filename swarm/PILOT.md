# Standalone pilot: start without sharing or migrating existing agents

This guide assumes an organization-approved Copilot environment, a disposable local feature branch or offline test repository, and no connection to a vehicle or live ROS graph. Use an offline, non-safety-critical component. All existing human and CI approvals remain required. Do not send internal material to external assistants or install unapproved integrations.

## A. Local project onboarding (read-only first)
1. Copy the new swarm profiles alongside existing profiles, not over them. They are self-contained; no old agent definitions are required.
2. Open the approved repository in VS Code, select the Copilot harness and confirm custom agents are discovered. Confirm actual permissions and supported model selections. Do not enable blanket terminal approval.
3. Paste `templates/project-discovery.md` into an approved read-only agent session. Save the proposed configuration locally only after reviewing it. Complete `PROJECT.md` yourself; unknown values remain TBD and block implementation. Keep proprietary files and outputs inside approved storage.
4. Resolve conflicts with repository instructions and team standards explicitly. Configure commands from actual existing CI/local build scripts, not invented defaults. Read `ROS2-VERIFICATION.md` and `VSCode-INTEGRATION.md`.

## B. Calibrate review before implementing anything
1. Select a previously human-reviewed, low-risk offline change. Obtain base/candidate SHAs, original story/requirements and approved test evidence. Use an isolated read-only checkout matching candidate and a saved immutable diff; no production data. If unavailable, mark evidence gaps rather than inventing them.
2. Select `swarm-orchestrator`, paste `templates/calibrate-review.md`, fill placeholders. Delegate only to independent read-only specialists. Do not expose human review findings or other reviewers' conclusions until all initial reports are returned. Include embedded/safety specialists when applicable, but do not use a safety-relevant story as the first pilot.
3. Compare against human findings after the reports are frozen. Do not treat historical findings as complete ground truth; adjudicate novel findings. Log missed findings, false positives, unsupported findings, review time and model identity using `templates/calibration-record.md`. Keep all records inside the approved environment.
4. Repeat on several representative low-risk changes, including at least one deliberately flawed synthetic change in a disposable offline repository. Compare models on the same snapshots and prompts where allowed. Record unresolved ambiguity and failures to delegate. Improve human-owned instructions based on recurring misses, not just one example.

## C. Shadow delivery
1. Select a new small, low-risk story; import it manually from ADO within the approved environment. No MCP required.
2. Launch with `templates/start-story.md`. Approve the specification, risk, allowed paths, exact plan and local commit authorization. Keep all incremental human gates.
3. At each candidate checkpoint: inspect evidence and the actual small diff, compare AI findings to your own, record feedback. Approve or reject the exact SHA. Confirm the orchestrator stops before starting another increment.
4. Verify final QA receives original requirements and whole-story diff, not just the incremental review summaries. Review the commit series and normal PR CI independently. No automatic pushes or merges.

## D. Decide whether to increase autonomy
Only after organization approval, stable tool boundaries, independent trusted enforcement and evidence of review quality, consider automatic progression of low-risk increments. Structural checker alone does not authorize progression. Keep significant design decisions, unknown risks, all safety-relevant changes and final approval human-gated. Set a maximum unattended increment budget (start with one or two), with exact SHA review packets and pause on deviation. Review all packets and commits before PR approval. Measure total human minutes per accepted story, not merely interruptions; compare against your old process and revert if results worsen.

There is no universal number of stories or acceptable miss rate. Your team's review and safety obligations determine whether changes are eligible. Keep all existing mandated verification, traceability and approval controls.
