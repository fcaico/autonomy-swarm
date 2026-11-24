Select swarm-orchestrator in Copilot. Supply a story card using your approved internal workflow.

Plan this story using the swarm workflow in shadow mode. Read swarm/PROJECT.md first. Use swarm-planner to produce a specification and bounded TDD plan. Save proposed artifacts under .swarm/runs/<story-id>/. Stop for my explicit approval before making any implementation change. Do not run tests or use commands not listed in the approved project adapter. After approval, invoke each specialist by name; do not self-review in place of a specialist. Stop after every increment and present its candidate SHA, test evidence, independent findings, decisions and unresolved gaps.

Resume prompt: Resume <story-id> from persisted state. I approve <exact spec/plan hashes or candidate SHA and next increment>, subject to the approved project adapter and shadow-mode gates. Do not reinterpret approval as release authorization.

For v0.2 include ADO work item ID/revision, feature branch, and project adapter status. Use `swarm/templates/increment-review.md` at each review gate. If existing agents are adopted, confirm the mapped role names and one-increment contract before delegation.
