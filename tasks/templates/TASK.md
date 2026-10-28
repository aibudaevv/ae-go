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

## Resume checkpoint

| Field | Value |
| --- | --- |
| Current unit | Unit 1 |
| Plan position | `PLAN.md`, Unit 1, Step 1 |
| Last completed | Task prepared; implementation not started |
| Last verification | Not run |
| Next action | Start Unit 1, Step 1 |
| Blocker | None |
| Uncommitted state | None |

Keep this checkpoint sufficient for another agent to resume in the same
workspace. Detailed instructions remain in `PLAN.md`; the full step-by-step
history remains in the temporary execution ledger.
