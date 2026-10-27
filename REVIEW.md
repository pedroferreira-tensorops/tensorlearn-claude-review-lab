# Review policy

Review against the approved requirements first (fixtures/approved.md), then the diff. Do not read the author narrative on the first pass; an author's claim is not evidence.

Every finding needs a path, a line in the changed file, a severity, the requirement it violates, quoted evidence from the diff and a recommendation. Severity: critical for privacy, authorization or release-gate violations; major for caching and rate-limit regressions; minor for style or generated files. Report at most one minor comment per patch. Report only findings the diff supports; if unsure, say what evidence is missing instead of guessing. Confirm a clean patch has no findings. The reviewer is read-only: it never edits, approves or merges.

```json
{
  "fixtures/defective.patch": {"findings": [
    {"path": "src/status.mjs", "line": 2, "severity": "major", "requirement": "SPEC-CACHE", "evidence": "const result = await service.get(code); drops cache.getOrLoad", "recommendation": "Restore the 30-second per-customer cache."},
    {"path": "src/status.mjs", "line": 3, "severity": "critical", "requirement": "SPEC-PRIVACY", "evidence": "console.info(result) logs the clinical payload", "recommendation": "Log the event ID and delivery state only."},
    {"path": "src/status.mjs", "line": 4, "severity": "critical", "requirement": "INTENT-1", "evidence": "result.reviewed || result.released", "recommendation": "Require reviewed && released."}
  ]},
  "fixtures/clean.patch": {"findings": []}
}
```
