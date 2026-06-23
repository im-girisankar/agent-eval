# agent-eval

**Know whether your agent actually works before shipping.**

agent-eval is a lightweight, pure-Python framework for evaluating LLM agent *trajectories* —
the full sequence of tool calls, observations, and reasoning steps — not just the final answer.

## Why trajectories?

A final answer can look correct while the agent hallucinated a tool call, skipped a required
step, or took ten times as many steps as needed. agent-eval surfaces these problems before
they reach production.

## Status — milestone roadmap

| Milestone | Scope | Status |
|-----------|-------|--------|
| **M1 — Core schema & metrics** | `ToolCall`, `Step`, `Run`, `ExpectedRun` dataclasses; four deterministic metrics | **Done** |
| **M2 — Harness & CLI** | JSON loader, `Report` object, markdown renderer, `ageval run <file>` CLI | **Done** |
| **M3 — Integrations & dashboards** | LangChain/OpenAI adapter, HTML report, CI diff-guard | Planned |

## Quick start

```bash
pip install -e ".[dev]"
ageval run examples/sample_runs.json
```

## Metrics

- **tool_call_correctness** — F1 of actual vs expected tool-call names (set-based).
- **goal_completion** — 1.0 if `goal_reached` is `True`, else 0.0.
- **step_efficiency** — `1.0` when `actual_steps <= expected_steps` (finishing early is fine),
  else `expected_steps / actual_steps`. Only meaningful steps (those containing a tool call)
  are counted, so reflection-only steps do not inflate the cost.
- **trajectory_score** — weighted composite: `0.4 * tool_call_correctness + 0.4 * goal_completion + 0.2 * step_efficiency`.

## License

MIT © 2026 Girisankar G
