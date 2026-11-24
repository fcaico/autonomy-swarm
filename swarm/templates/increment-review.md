# Increment <ID> — review packet
ADO story ID/revision: <...> | Requirement IDs: <...>
Base SHA / candidate SHA: <...> / <...>
Changed package(s), production/test lines and paths: <...>

## What changed and why
<2–4 sentences, design choices, interface/DI decisions and rationale; explicit deviations>

## Human attention
- Decision required / unresolved findings / uncertainty: <... or explicitly none>
- Suggested review order: <file:line, why important>
- Diff command: `git diff <base> <candidate>` (exact SHAs)

## Verification evidence
| Check | Exact command and snapshot | Observed result / count | Log or XML |
|---|---|---|---|
| RED | ... | named expected assertion failure | ... |
| GREEN | ... | same test passes | ... |
| REFACTOR / regression | ... | ... | ... |
Host integration, CI and target status: <verified / pending / not applicable with approved rationale>

## Independent reviews
| Role / model if known | Candidate | Verdict | Report path | Open findings |
|---|---|---|---|---|
| cpp | ... | ... | ... | ... |
| test | ... | ... | ... | ... |
Include embedded/safety as required, all disagreements and dispositions. No invented approvals or test evidence.

## Approval (human only)
Shadow: stop and request approval of exact candidate and next increment. Keep original report and approval separate; later edits invalidate candidate review.
