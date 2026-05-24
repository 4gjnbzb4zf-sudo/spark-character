<!-- sentinel:link-proof -->
### 🔗 Link Proof

_Click through to see exactly what this PR changes, without rebuilding the hunt context._

- **Offending line (upstream)**: [evals/lowest_tier_watch.py:104](https://github.com/vibeforge1111/spark-character/blob/master/evals/lowest_tier_watch.py#L104)
- **Severity**: 🟢 LOW
- **Finding ID**: `SILE-392`
- **Category**: `silent-failure`
- **Detector**: `silent-failure` (sentinel-engine)
- **Discovered**: 2026-05-24

<!-- sentinel:link-proof -->

## Surface the swallowed exception in lowest tier watch eval so operators can trace it

The exception handler at <code>evals/lowest_tier_watch.py:104</code> catches the error and returns a failure-shaped result without logging. If this fires in production, you'll never see it in the application's own logs — only the downstream consumer notices, usually as silent data loss.

### 🔴 Before

`evals/lowest_tier_watch.py:104`

```python
def _write_heartbeat(path: Path, phase: str) -> None:
    try:
        path.parent.mkdir(parents=True, exist_ok=True)
        path.write_text(f"{int(time.time())} {phase}\n", encoding="utf-8")
    except Exception:
        pass
```

### 🟢 After

```python
def _write_heartbeat(path: Path, phase: str) -> None:
    try:
        path.parent.mkdir(parents=True, exist_ok=True)
        path.write_text(f"{int(time.time())} {phase}\n", encoding="utf-8")
    except Exception as exc:
        print(f"[lowest_tier_watch] heartbeat write failed: {exc}", flush=True)
```

### 🔬 Evidence

| Field | Value |
|---|---|
| File | `evals/lowest_tier_watch.py:104` |
| Category | `silent-failure` |
| Severity | 🟢 LOW |
| Detector | `silent-failure` |
| Discovered | 2026-05-24 |

---
Single-purpose change. Compile-verified locally; behavior unchanged for the success path.
