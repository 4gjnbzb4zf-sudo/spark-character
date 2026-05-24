<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [evals/score_trend.py:48](https://github.com/vibeforge1111/spark-character/blob/master/evals/score_trend.py#L48)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-510`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Log the score trend eval failure instead of swallowing it

The exception handler at <code>evals/score_trend.py:48</code> catches the error and returns a failure-shaped result without logging. If this fires in production, you'll never see it in the application's own logs — only the downstream consumer notices, usually as silent data loss.

### 🔴 Before

`evals/score_trend.py:48`

```python
            try:
                rows.append(json.loads(line))
            except json.JSONDecodeError:
                continue
```

### 🟢 After

```python
            try:
                rows.append(json.loads(line))
            except json.JSONDecodeError as e:
                print(f"warning: skipping malformed line in {history_path}: {e}", file=sys.stderr)
                continue
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `evals/score_trend.py:48` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
No behavioral change beyond the surfaced log line. Happy to split this if you'd prefer separate PRs per call site.
