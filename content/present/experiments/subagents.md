# Experiments · Sub-Agents

## Productivity Benchmark: Linear vs. Subagents

*Date:* 2026-04-28
*Model:* Claude Sonnet 4.6
*Task:* Build a Flask REST API for a Library Management System (`models.py`, `app.py`, `test_api.py`)

## Methodology

The experiment compared two code-generation modes for the same task:

- *Round 1, Linear:* all files written sequentially by the main agent in a single reasoning pass, with no delegation.
- *Round 2, Subagents:* 3 subagents launched in parallel, each responsible for one file, with full context provided by the orchestrating agent.

Both rounds ran in the same environment (Arch Linux, Python 3.14.3, Flask + pytest via virtualenv) and were validated with `python -m pytest test_api.py -v`.

## Metrics Collected

| Metric | Round 1, Linear | Round 2, Subagents |
| --- | --- | --- |
| Code generation time | ~15 s | ~18 s (wall-clock, parallel) |
| Total tokens (input + output) | ~11.200 (estimated¹) | *29.493* (measured via runtime) |
| Tool calls, code | 3 `Write` | 4 `Write` (distributed) |
| Tool calls, total | 9 (3 Write + 6 Bash²) | 12 (3 Agent + 4 Write + 2 Read + 3 Bash) |
| Parallel execution | No | Yes (3 simultaneous agents) |
| Tests passing | 3 / 3 | 3 / 3 |
| Errors / rework | 0 | 0 |

*¹ Estimated: the main agent's context includes the system prompt (~6 k tokens) + conversation history + the contents of the 3 files. Subagent tokens are not exposed by the runtime in Round 1.*

*² The 6 Bash calls include failed dependency-installation attempts (the managed Arch Linux environment requires `python -m venv`).*

## Round 2 Breakdown by Subagent

| Subagent | File | Total Tokens | Tools | Duration |
| --- | --- | --- | --- | --- |
| models-agent | `models.py` | 9.246 | 1 Write | 7.410 ms |
| api-agent | `app.py` | 9.711 | 1 Write | 8.546 ms |
| test-agent | `test_api.py` | 10.536 | 2 Write/Edit | 18.009 ms |
| *Total* | n/a | *29.493* | *4* | *18.009 ms* (bottleneck: test-agent) |

Round 2's wall-clock time is dictated by the slowest subagent (test-agent, 18 s). The other two finished in ~7–8 s but sat idle waiting for the bottleneck to complete.

## Qualitative Code Analysis

### `models.py`, identical across both rounds

Both produced the same dataclass with `asdict`:

```python
from dataclasses import dataclass, asdict

@dataclass
class Book:
    id: int
    titulo: str
    autor: str
    ano: int

    def to_dict(self) -> dict:
        return asdict(self)
```

### `app.py`, stylistic difference in the decorators

```python
# Round 1: modern Flask 2.0+ decorators (more concise)
@app.get("/books")
@app.post("/books")
@app.delete("/books/<int:book_id>")

# Round 2: traditional style with @app.route (more explicit)
@app.route("/books", methods=["GET"])
@app.route("/books", methods=["POST"])
@app.route("/books/<int:book_id>", methods=["DELETE"])
```

The internal logic (the `_find_book` helper, field validation, HTTP returns) was equivalent across both rounds.

### `test_api.py`, Round 2 produced more robust tests

```python
# Round 1: whole-object comparison (fragile to field order)
assert r_get.get_json() == created

# Round 2: granular per-field assertions (more resilient and readable)
assert data["titulo"] == "Dom Casmurro"
assert data["autor"] == "Machado de Assis"
assert data["ano"] == 1899
```

Round 2 also used more descriptive variable names (`post_response`, `get_response`, `delete_response` vs. `r_post`, `r_get`, `r_del`) and its reset fixture was leaner (no unnecessary `yield`).

## Test Results

Both rounds produced functional, error-free code:

```text
============================= test session starts ==============================
platform linux -- Python 3.14.3, pytest-9.0.3
collected 3 items

test_api.py::test_get_all_books_empty        PASSED  [ 33%]
test_api.py::test_create_and_get_book        PASSED  [ 66%]
test_api.py::test_delete_book                PASSED  [100%]

============================== 3 passed in 0.06s ===============================
```

## Final Comparison Table

| Dimension | Winner | Rationale |
| --- | --- | --- |
| *Token efficiency* | Round 1 | ~2,6× fewer tokens (no per-subagent context overhead) |
| *Speed (wall clock)* | Tie | R1 ~15 s generation; R2 ~18 s (bottleneck in test-agent) |
| *Test quality* | Round 2 | Granular assertions, descriptive names, cleaner fixture |
| *Scalability* | Round 2 | Real parallelism benefits larger, independent tasks |
| *Operational simplicity* | Round 1 | No orchestration overhead or risk of inconsistency across agents |

## Conclusion

The token overhead in Round 2 (~18.300 extra tokens) is the fixed cost of spinning up 3 isolated contexts with a full briefing. For a task of this scale (3 small files, <200 lines in total), that cost does not pay off in speed: the test-agent was the bottleneck and made Round 2 slightly slower than the linear approach.

The subagent pattern starts to pay off when:

1. The files or modules are substantially larger (CPU time per subagent > 30 s).
2. The dependencies between subtasks are minimal (each agent can work truly independently).
3. The quality of the individual output matters more than the total token cost (subagents tend to produce more careful code because they focus on a smaller scope).

For small, cohesive tasks like this one, the linear approach is more token-efficient and just as fast, with the added advantage of naturally maintaining stylistic consistency, with no need for briefing between agents.
