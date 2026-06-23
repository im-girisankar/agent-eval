# agent-eval — Claude notes

## Project layout

```
src/agent_eval/   core library (schema, metrics, harness, cli)
tests/            pytest suite (15+ tests, fully offline)
examples/         sample JSON runs + gold spec
.ci/ci.yml        parked CI config (not under .github/)
```

## Key invariants

- **Zero runtime dependencies.** All imports are stdlib only; no network calls.
- **Deterministic.** Every metric is a pure function — same input, same float output.
- **Ruff-clean.** `ruff check src tests` must pass with the config in pyproject.toml.

## Running locally

```bash
pip install -e ".[dev]"
ruff check src tests
pytest
ageval run examples/sample_runs.json
```

## Extending

- Add a new metric in `src/agent_eval/metrics.py` and wire it into `trajectory_score`.
- Add a new loader format in `src/agent_eval/harness.py` (`load_runs_from_dict`).
