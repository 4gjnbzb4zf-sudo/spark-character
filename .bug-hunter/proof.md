<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [evals/compare_personas.py:157](https://github.com/vibeforge1111/spark-character/blob/master/evals/compare_personas.py#L157)
- **Severity**: 🟡 MEDIUM
- **Finding ID**: `ERRO-779`
- **Category**: `error-message-actionability`
- **Detector**: `error-message-actionability` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Give the compare personas eval error message a recovery hint

The error at <code>evals/compare_personas.py:157</code> names what went wrong but not what to do about it. An operator hitting this message has to read source to figure out the fix — which is exactly the kind of friction that turns minor failures into hour-long debug sessions.

### 🔴 Before

`evals/compare_personas.py:157`

```python
    print("\n=== verdict ===")
```

### 🟢 After

```python
    print("\n=== verdict (higher composite wins; see delta below) ===")
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `evals/compare_personas.py:157` |
| Category | `error-message-actionability` |
| Severity | 🟡 MEDIUM |
| Detector | `error-message-actionability` |
| Discovered | 2026-05-24 |

---
Tested with the existing test fixtures; nothing regressed. Drop the proof commit before merging if you don't want the <code>.bug-hunter/</code> directory in <code>main</code>.
