# `task-NNN`: Short title

| Field | Value |
| --- | --- |
| ID | `task-NNN` |
| Status | `Draft` |
| Created | `YYYY-MM-DD` |
| Workspace | Worktree path or current checkout |

## Artifacts

- [Design](DESIGN.md)
- [Grilling](GRILLING.md)
- [Plan](PLAN.md)

## Units

| Unit | Result | Status | Evidence |
| --- | --- | --- | --- |
| 1 | Independently reviewable outcome | `Pending` | Not run |

Allowed unit statuses: `Pending`, `In Progress`, `Blocked`, and `Done`. `Done`
is effective only after the permitted unit commit succeeds. Find unit commits
through their `Task` and `Unit` Git trailers rather than storing hashes here.

## Acceptance results

Reference criteria from `DESIGN.md` without copying their requirement text.

| Criterion | Status | Evidence |
| --- | --- | --- |
| `AC-1` | `Pending` | Not run |

## Limitations

- Record unavailable checks, known gaps, and accepted constraints.

## Blocker and next action

State the blocker or the single next action. Detailed steps belong in
`PLAN.md`; step-level progress belongs in the temporary execution ledger.
