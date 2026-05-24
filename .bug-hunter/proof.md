<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [evals/compare_personas.py:89](https://github.com/vibeforge1111/spark-character/blob/master/evals/compare_personas.py#L89)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-728`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Surface the swallowed exception in compare personas eval so operators can trace it

This <code>except</code> block at <code>evals/compare_personas.py:89</code> drops the exception and continues. Operators investigating a failure read every log line in the path and find nothing. The clue lives only in the caller's return value, which the caller usually treats as fine.

### 🔴 Before

`evals/compare_personas.py:89`

```python
            t6_scores.append(run_deep_probe(probe, provider=provider, persona=persona, max_tokens=max_tokens).score)
        except Exception:
            pass
```

### 🟢 After

```python
            t6_scores.append(run_deep_probe(probe, provider=provider, persona=persona, max_tokens=max_tokens).score)
        except Exception as exc:
            print(f"  T6 probe error on {probe.id}: {exc}")
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `evals/compare_personas.py:89` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
Tested with the existing test fixtures; nothing regressed. Drop the proof commit before merging if you don't want the <code>.bug-hunter/</code> directory in <code>main</code>.
