# Prompt: review-only calibration (no implementation)
Mode: calibration, read-only; no changes, commits, builds, tests, external data transfer, deployment or tool permission escalation.
Story and revision: [approved local reference]
Base SHA: [full SHA]
Candidate SHA: [full SHA]
Immutable diff and evidence paths: [approved local paths]
Relevant project instructions: [approved local paths]

Read WORKFLOW.md and PROJECT.md. Confirm the supplied snapshot, immutable diff and evidence are available; otherwise stop with gaps. Use actual native subagent delegation to swarm-cpp-reviewer and swarm-test-reviewer independently, and any additional required specialist. Give every reviewer the same original requirements, diff and relevant code/evidence. Do not pass prior human findings, other review conclusions or contributor rationale before independent analysis. Do not edit or run commands; ask the human to supply missing snapshots/evidence. Verify each agent's actual tool permissions; stop on failure. Return raw reports, exact SHA, model if known and a consolidated report with evidence citations, dissent and gaps. Do not attempt fixes. Stop for human comparison. A pass is not human or safety approval. No evidence checker needed for calibration.
