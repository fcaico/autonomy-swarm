# Copilot ROS 2 autonomy delivery swarm — v0.3
Customized for VS Code / GitHub Copilot, C++ ROS 2 / colcon / Google Test / Google Mock, Linux host development and Jetson Thor target, Azure DevOps work items and GitHub feature branches/Actions. Uses manual dependency composition (no DI container). Starts in shadow mode. No target or live vehicle access.

## Start here
Read `swarm/PILOT.md` for a standalone, read-only calibration and shadow-delivery pilot. Existing agent files are not required; keep them intact until you have compared results. `swarm/templates/project-discovery.md` lets your approved Copilot environment propose a local configuration without sharing confidential data.

## Install
- Review `swarm/VSCode-INTEGRATION.md` first; copy `.github/agents/swarm-*.agent.md`, optional `.github/prompts/swarm-start.prompt.md`, and `swarm/` into a disposable approved feature branch. Check conflicts. Do not replace your existing agents blindly.
- Complete and approve `swarm/PROJECT.md`; it intentionally leaves actual build commands, toolchain, repo paths and policy TBD. Add `.swarm/runs/` to local Git exclude or approved ignore list. Use isolated environment without vehicle connectivity or production credentials.
- Verify the agents and actual tool permissions in the Copilot harness. Select `swarm-orchestrator` and use `swarm/templates/start-story.md` or the optional prompt file. Provide story ID/revision and text. Approve exact plan and every candidate increment in shadow mode.
- See `swarm/ROS2-VERIFICATION.md` for example colcon test workflow and `swarm/templates/increment-review.md` for a human-facing digest. Existing CI remains authoritative. Optional ADO remote MCP example is read-only and not enabled by default.
- Compare independent reviewer findings against your own. Keep mandated reviews; do not switch to pilot without independently enforced gates and team authorization.

## Tests and limitations
`cd swarm && python3 -m unittest -v test_check_gate` runs the original structural checker tests. The checker validates claims, not provenance, actual tests, risk or approval. This package is a prompt-driven prototype and templates, not a deterministic runtime. No live VS Code/Copilot, ROS workspace, GitHub Actions or ADO integration has been tested. Verify tool availability, model selection, actual profile delegation and read-only enforcement. No automated target, merge, push, deployment or release action. The optional prompt file assumes the orchestrator is selected. Existing project-owned standards and safety processes take precedence. The starter profiles are standalone replacements for a pilot, not a migration requirement. Existing profiles were not supplied; preserve them until the pilot establishes a benefit. Do not use as safety/release assurance.

See integration and verification guides for official documentation links.
