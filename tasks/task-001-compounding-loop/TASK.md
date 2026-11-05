# `task-001`: Add a compounding learning loop

| Field | Value |
| --- | --- |
| ID | `task-001` |
| Status | `In Progress` |
| Created | `2026-11-04` |
| Workspace | Current checkout: `/home/aleksey/go/src/github.com/ae-go` |

## Artifacts

- [Design](DESIGN.md)
- [Grilling](GRILLING.md)
- [Plan](PLAN.md)

## Units

| Unit | Result | Status | Evidence |
| --- | --- | --- | --- |
| 1 | Canonical learning lifecycle and improvement register | `Done` | RED searches failed before policy; GREEN searches, links, whitespace, and diff checks passed |
| 2 | Agent routing, templates, and repository orientation | `Pending` | Not run |

Allowed unit statuses: `Pending`, `In Progress`, `Blocked`, and `Done`. `Done`
is effective only after the permitted unit commit succeeds. Find unit commits
through their `Task` and `Unit` Git trailers rather than storing hashes here.

## Acceptance results

| Criterion | Status | Evidence |
| --- | --- | --- |
| `AC-1` | `Pending` | Not run |
| `AC-2` | `Pending` | Not run |
| `AC-3` | `Pending` | Not run |
| `AC-4` | `Pending` | Not run |
| `AC-5` | `Pending` | Not run |
| `AC-6` | `Pending` | Not run |
| `AC-7` | `Pending` | Not run |

## Limitations

- Version 1 is Markdown-only and adds no scripts, CI, executable evals, or Go
  service code.

## Resume checkpoint

| Field | Value |
| --- | --- |
| Current unit | Unit 1 in progress |
| Plan position | `PLAN.md`, Unit 1 Step 6 |
| Last completed | Unit 1 verified and prepared as `Done`; effectiveness awaits commit success |
| Last verification | Required-term searches, Mc/DC inspection, local links, whitespace, and `git diff --check` passed |
| Next action | Create the permitted Unit 1 commit |
| Blocker | None |
| Uncommitted state | Complete verified Unit 1 with prepared `Done` state |
