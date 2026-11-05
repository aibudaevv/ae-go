# `<TASK-ID>`: Short title

| Field | Value |
| --- | --- |
| ID | `<TASK-ID>` |
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

## Compounding review

Complete this section before setting the task to `Completed`. Use `No action`,
`Promoted`, or `Deferred` according to the lifecycle in `tasks/README.md`.

| Signal reviewed | Outcome | Evidence, canonical target, or improvement ID |
| --- | --- | --- |
| No material signal recorded yet | `Pending` | Review before completion |

## Limitations

- Record unavailable checks, known gaps, and accepted constraints.

## Resume checkpoint

| Field | Value |
| --- | --- |
| Current unit | Not selected |
| Plan position | `DESIGN.md`, initial draft |
| Last completed | Task manifest initialized; design not started |
| Last verification | Not run |
| Next action | Draft `DESIGN.md` through the brainstorming workflow |
| Blocker | None |
| Uncommitted state | Initial task artifacts |

Keep this checkpoint sufficient for another agent to resume in the same
workspace. Detailed instructions remain in `PLAN.md`; the full step-by-step
history remains in the temporary execution ledger.
