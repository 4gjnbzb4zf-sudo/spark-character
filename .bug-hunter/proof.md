<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [evals/cross_provider.py:229](https://github.com/vibeforge1111/spark-character/blob/master/evals/cross_provider.py#L229)
- **Severity**: 🟡 MEDIUM
- **Finding ID**: `ERRO-772`
- **Category**: `error-message-actionability`
- **Detector**: `error-message-actionability` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Give the cross provider eval error message a recovery hint

This error message at <code>evals/cross_provider.py:229</code> is technically correct but operator-hostile — it surfaces the policy without the recovery step. Adding a one-line hint saves a doc-spelunking detour for every user who hits it.

### 🔴 Before

`evals/cross_provider.py:229`

```python
    print("\nsame-agent judge across pairs:\n")
```

### 🟢 After

```python
    print("\nsame-agent judge across pairs (0-10 per prompt; pair mean reported as 0-1, aim for >=0.7):\n")
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `evals/cross_provider.py:229` |
| Category | `error-message-actionability` |
| Severity | 🟡 MEDIUM |
| Detector | `error-message-actionability` |
| Discovered | 2026-05-24 |

---
No behavioral change beyond the surfaced log line. Happy to split this if you'd prefer separate PRs per call site.
