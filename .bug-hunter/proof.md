<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [evals/compare_personas.py:82](https://github.com/vibeforge1111/spark-character/blob/master/evals/compare_personas.py#L82)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-399`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Stop dropping compare personas eval errors without a log line

This <code>except</code> block at <code>evals/compare_personas.py:82</code> drops the exception and continues. Operators investigating a failure read every log line in the path and find nothing. The clue lives only in the caller's return value, which the caller usually treats as fine.

### 🔴 Before

`evals/compare_personas.py:82`

```python
            try:
                t2_scores.append(score_distinctiveness(r.final, provider=provider).score)
            except Exception:
                pass
```

### 🟢 After

```python
            try:
                t2_scores.append(score_distinctiveness(r.final, provider=provider).score)
            except Exception as exc:
                print(f"  t2 score error on {prompt[:40]!r}: {exc}")
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `evals/compare_personas.py:82` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
No behavioral change beyond the surfaced log line. Happy to split this if you'd prefer separate PRs per call site.
