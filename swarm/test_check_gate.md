# Sample test_check_gate

```python
import copy
import unittest
from check_gate import assess

SHA = "a" * 40

def packet():
    return dict(snapshot=SHA, risk="low", mode="pilot", embedded_relevant=False,
                safety_relevant=False, escalations=[],
                reviews=[dict(role=r, snapshot=SHA, verdict="pass", findings=[], gaps=[]) for r in ("cpp", "test")],
                checks={k: dict(candidate=SHA, status="expected_failure_observed" if k == "red" else "pass", log=k+".txt") for k in ("red", "green", "refactor", "host_regression")})

class GateTest(unittest.TestCase):
    def test_clean_structure(self): self.assertEqual(assess(packet()), [])
    def test_shadow(self):
        p=packet(); p["mode"]="shadow"; self.assertTrue(assess(p))
    def test_stale(self):
        p=packet(); p["reviews"][0]["snapshot"]="b"*40; self.assertTrue(assess(p))
    def test_missing_required_reviewer(self):
        p=packet(); p["embedded_relevant"]=True; self.assertTrue(assess(p))
    def test_failed_check(self):
        p=packet(); p["checks"]["red"]["status"]="pass"; self.assertTrue(assess(p))
    def test_dissent(self):
        p=packet(); p["reviews"][1]["findings"]=[{"severity":"major"}]; self.assertTrue(assess(p))
    def test_malformed(self): self.assertTrue(assess([])); self.assertTrue(assess({}))
    def test_unknown_risk(self):
        p=packet(); p["risk"]="unknown"; self.assertTrue(assess(p))
    def test_unknown_relevance(self):
        p=packet(); del p["safety_relevant"]; self.assertTrue(assess(p))

if __name__ == "__main__": unittest.main()
```