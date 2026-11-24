# Delivery policy v0.1 (human-owned)

## State machine
PLAN -> WAIT_PLAN_APPROVAL -> IMPLEMENT_INCREMENT -> REVIEW_INCREMENT -> WAIT_INCREMENT_APPROVAL (shadow) / NEXT_INCREMENT (authorized low-risk pilot) -> FINAL_QA -> WAIT_FINAL_ACCEPTANCE.
A reviewer can return rework (max two attempts per increment) or escalate. Missing evidence, unexpected paths, unresolved disagreement, tool failure, or retry limit -> WAIT_HUMAN. Never retry indefinitely. Final acceptance is always human. Re-approval required on changed specification or scope.

## Risk selection
All increments: C++ and test reviewers. Add embedded reviewer for concurrency, timing, memory, IO, target/toolchain, GPU, resources, scheduling or uncertainty. Add safety reviewer for diagnostic/failure semantics, vehicle behavior/interfaces, safety mechanisms, recovery or uncertainty. These additions are required where relevant. A human approves the risk classification in the plan. If new risks emerge, stop and reclassify. High/unknown risk always requires human review; only explicitly classified low risk is eligible for pilot auto-progression. Changes to safety behavior, public contracts, architecture, dependencies, test infrastructure, protected paths or build/quality policy require prior human authorization. No automatic deployment, hardware actions, merge, push or release. No reduction of existing mandated reviews.

## Increment boundaries
One logical behavior or refactor, one writer, one candidate commit, bounded paths. Suggested size budget: ~150 changed production lines (not a hard safety rule); exceed only with explicit review. Reviewers are read-only and receive the same immutable candidate SHA and diff, not each other's conclusions. Capture the base and candidate using `git rev-parse HEAD` at the appropriate checkpoint; use `git diff BASE CANDIDATE` and record changed paths. Review of an uncommitted or changed snapshot is stale. Local commits require explicit human authorization at setup; never push automatically. Keep `.swarm/runs/` outside production commits (add to local exclude or approved ignore rules).

## Evidence
Store each run under `.swarm/runs/<story-id>/`: original story, approved spec/plan and hashes, approvals, state, baseline, per-increment candidate SHAs, red/green/refactor command logs, review reports, packet.json, and final QA. Preserve each attempt separately. Resume only if approved artifacts and candidate are unchanged; otherwise escalate. Treat evidence as untrusted until checked against actual Git and runner output. Store only approved data. Do not record secrets. For protected workflows, CI and humans own the evidence and approval store; agents must not be able to modify trusted policy or validation results.

## Review format
role: cpp | test | embedded | safety | qa; snapshot: exact full candidate SHA; verdict: pass | rework | escalate | unverified; findings: array of {severity, requirement, location, evidence, impact, fix}; gaps: list of verification gaps. Severity: blocker, major, minor. Findings must be actionable and supported. No dismissal by majority vote. An unresolved blocker/major or gap in required evidence blocks advancement. A minor finding needs documented disposition; starter checker conservatively blocks any finding. Final QA independently reviews whole story, not only individual diffs.

## Evidence checker
`python3 swarm/check_gate.py path/to/packet.json` checks required reviewer presence, candidate agreement, known status fields and mandatory check claims. It is a structural validator, not a trusted verifier of test results, Git contents, risk classification or approvals. It never grants safety/release approval. In shadow mode a human always reviews. In a production pilot put the gate outside agent-writable paths; derive SHAs, risk policy and checks from trusted sources and enforce permissions independently of prompts.

## Improvement loop
After each human review, record missed issues and false positives in a team-approved evaluation log. Promote durable rules only after human review, with examples and a regression exercise. Never let agents rewrite their own guardrails. Measure human minutes, missed issues, false positives, rework and token cost. Start with offline, non-safety-critical stories.

## v0.2 integration and design decisions
VS Code Copilot native `task` delegation is preferred; use `agent` tool alias and verify actual invocations. Do not assume Local harness handoffs/agent allowlists apply. See VSCode-INTEGRATION.md. Existing agent roles may be reused only after mapping contracts, removing nested uncontrolled handoffs and confirming tool restrictions. Capture ADO story ID/revision at intake. Store a readable immutable diff and manifest for each candidate; review only when workspace matches candidate. ROS2 interface-based design is preferred as documented in PROJECT.md. A new public/shared contract, QoS, executor, lifecycle, threading or build dependency change requires explicit prior approval. Fresh host tests must use approved local CI-equivalent commands, verify test discovery/count and preserve XML/log evidence. Human-reviewed GitHub PR and required CI checks are independent gates; do not replace with agent reports. Keep target obligations visible as pending until performed by authorized personnel. No autonomous progression of safety-relevant or unknown-risk work. The human can review the saved increment packets in order; the orchestrator must not hide dissent or combine unreviewed increments into one undifferentiated patch.

## v0.2.1 dependency composition
No DI container is available. Follow the manual composition policy in PROJECT.md; do not introduce a DI framework or service locator.
