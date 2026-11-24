# Project adapter — human-owned, v0.3
Known environment:
- IDE: VS Code, GitHub Copilot; custom `.agent.md` profiles.
- C++ ROS 2 network on Linux; native developer-host verification; deployment target NVIDIA Jetson Thor.
- colcon builds; Google Test and Google Mock; SOLID, interface-based design and dependency injection encouraged.
- Azure DevOps story cards, GitHub feature branches and GitHub Actions CI/CD.

Complete and approve the following before implementation. TBD is not authorization.
- Repo/component, ROS distribution, C++ standard, host OS/architecture/compiler, target SDK/OS/toolchain: TBD.
- Agent set: standalone swarm profiles by default; existing profile mapping optional. Confirm compatibility with project policy locally.
- Workspace setup (underlay, overlay, dependencies, environment) and exact approved commands with cwd: TBD.
  - targeted build / targeted behavioral test / fresh test-result evidence: TBD
  - host regression and local CI equivalent: TBD
  - static analysis, lint and integration tests: TBD
  - target build and controlled target test procedure/owner: TBD
- Build and test output isolation; approved ROS graph isolation and test data: TBD.
- Architecture docs, interface conventions, coding standards and existing requirements/safety artifacts: TBD paths.
- Feature branch convention, baseline, authorized local commit scope, protected paths, required PR checks/reviewers: TBD.
- Approved data/model policies, work item acquisition, evidence retention and escalation owners: TBD.
- Mode: shadow. All human gates remain. No push, merge, deployment or hardware access by agents.

Use existing local CI entrypoints wherever possible; do not invent commands or replace CI. No live ROS graph, physical vehicle, production credentials or unrestricted shell approval. A sandbox with approved local dependencies and no physical control connectivity is required. See `swarm/ROS2-VERIFICATION.md` and `swarm/VSCode-INTEGRATION.md`.

## Dependency composition policy
No dependency injection container is available. Prefer explicit constructor injection of narrow interfaces, with manual composition at existing application/node boundaries and clear lifetime/ownership semantics. Do not introduce DI frameworks, service locators or new dependencies. Preserve established interfaces even when only one production implementation exists; do not create interfaces indiscriminately. Changes to shared/public contracts or composition architecture require approval.
