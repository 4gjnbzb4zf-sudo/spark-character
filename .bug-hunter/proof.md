<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [evals/score_trend.py:133](https://github.com/vibeforge1111/spark-character/blob/master/evals/score_trend.py#L133)
- **Severity**: 🟡 MEDIUM
- **Finding ID**: `ERRO-977`
- **Category**: `error-message-actionability`
- **Detector**: `error-message-actionability` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Make the score trend eval error tell the user what to do next

The error at <code>evals/score_trend.py:133</code> names what went wrong but not what to do about it. An operator hitting this message has to read source to figure out the fix — which is exactly the kind of friction that turns minor failures into hour-long debug sessions.

### 🔴 Before

`evals/score_trend.py:133`

```python
    print("\n=== summary (per tier, over the window) ===\n")
```

### 🟢 After

```python
    print("\n=== summary (per tier, over the window) ===")
    print("(^ improvement, v regression vs --compare-back ago; rerun evals/continuous_eval.py to extend the window)\n")
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `evals/score_trend.py:133` |
| Category | `error-message-actionability` |
| Severity | 🟡 MEDIUM |
| Detector | `error-message-actionability` |
| Discovered | 2026-05-24 |

---
No behavioral change beyond the surfaced log line. Happy to split this if you'd prefer separate PRs per call site.
