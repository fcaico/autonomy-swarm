# Sample check_gate

```python
#!/usr/bin/env python3
"""Structural review-packet check; does not authenticate evidence or approvals."""
import json
import re
import sys

SHA = re.compile(r"(?:[0-9a-f]{40}|[0-9a-f]{64})\Z")
ROLES = {"cpp", "test", "embedded", "safety"}
CHECKS = {"red", "green", "refactor", "host_regression"}

def assess(packet):
    if not isinstance(packet, dict):
        return ["packet must be an object"]
    errors = []
    snapshot = packet.get("snapshot")
    if not isinstance(snapshot, str) or not SHA.fullmatch(snapshot):
        errors.append("invalid candidate SHA")
    if packet.get("risk") != "low":
        errors.append("risk not low; human decision required")
    if packet.get("mode") != "pilot":
        errors.append("not pilot mode; human review required")
    for flag in ("embedded_relevant", "safety_relevant"):
        if type(packet.get(flag)) is not bool:
            errors.append(f"{flag} must be explicitly true or false")
    required = {"cpp", "test"}
    if packet.get("embedded_relevant") is not False: required.add("embedded")
    if packet.get("safety_relevant") is not False: required.add("safety")
    reports = packet.get("reviews")
    if not isinstance(reports, list):
        return errors + ["missing reviews array"]
    seen = set()
    for report in reports:
        if not isinstance(report, dict):
            errors.append("malformed review"); continue
        role = report.get("role")
        if not isinstance(role, str) or role not in ROLES:
            errors.append("unknown review role"); continue
        if role in seen: errors.append(f"duplicate {role} review")
        seen.add(role)
        if report.get("snapshot") != snapshot: errors.append(f"stale {role} review")
        if report.get("verdict") != "pass": errors.append(f"{role} did not pass")
        if report.get("findings") != [] or report.get("gaps") != []:
            errors.append(f"{role} findings or gaps unresolved/missing")
    for role in sorted(required-seen): errors.append(f"missing {role} review")
    checks = packet.get("checks")
    if not isinstance(checks, dict): return errors + ["missing checks object"]
    for name in sorted(CHECKS):
        check = checks.get(name)
        if not isinstance(check, dict):
            errors.append(f"missing {name} evidence"); continue
        expected = "expected_failure_observed" if name == "red" else "pass"
        if check.get("status") != expected: errors.append(f"{name}: unexpected status")
        if check.get("candidate") != snapshot: errors.append(f"{name}: candidate association missing/stale")
        if not isinstance(check.get("log"), str) or not check["log"].strip():
            errors.append(f"{name}: missing log reference")
    if packet.get("escalations") != []: errors.append("escalations unresolved/missing")
    return errors

def main():
    if len(sys.argv) != 2:
        print("usage: check_gate.py packet.json", file=sys.stderr); return 2
    try:
        with open(sys.argv[1], encoding="utf-8") as stream:
            errors = assess(json.load(stream))
    except (OSError, ValueError) as exc:
        print(f"BLOCK: {exc}"); return 2
    if errors:
        print("HUMAN REVIEW / BLOCK:")
        for error in errors: print(f"- {error}")
        return 1
    print("STRUCTURE OK ONLY — validate trusted evidence and approval policy separately.")
    return 0

if __name__ == "__main__":
    sys.exit(main())
```