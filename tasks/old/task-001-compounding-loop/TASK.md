# `task-001`: Add a compounding learning loop

| Field | Value |
| --- | --- |
| ID | `task-001` |
| Status | `Completed` |
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
| 2 | Agent routing, templates, and repository orientation | `Done` | RED routing checks, GREEN routing/template checks, whole-task review, review-fix RED→GREEN pass, and final verification passed |

Allowed unit statuses: `Pending`, `In Progress`, `Blocked`, and `Done`. `Done`
is effective only after the permitted unit commit succeeds. Find unit commits
through their `Task` and `Unit` Git trailers rather than storing hashes here.

## Acceptance results

| Criterion | Status | Evidence |
| --- | --- | --- |
| `AC-1` | `Passed` | Lifecycle and task template require a compounding review before completion. |
| `AC-2` | `Passed` | Improvement register defines IDs, evidence sources, triggers, targets, decisions, and statuses. |
| `AC-3` | `Passed` | Policy distinguishes one ordinary source, one serious risk, and two independent sources. |
| `AC-4` | `Passed` | Design and grilling require review of relevant open register entries. |
| `AC-5` | `Passed` | Policy requires one canonical owner, evidence threshold, and no silent scope expansion. |
| `AC-6` | `Passed` | Root instructions route both design-time and completion-time workflow triggers. |
| `AC-7` | `Passed` | Small work remains exempt; final scope has no Go code, dependencies, scripts, CI, or eval runner. |

## Compounding review

| Signal reviewed | Outcome | Evidence, canonical target, or improvement ID |
| --- | --- | --- |
| Superpowers inline helpers differed from their documented path and required `Task N` headings rather than the repository's `Unit N` terminology. | `Deferred` | `task-001-I1` in the repository's `tasks/IMPROVEMENTS.md`; one independent source, so it remains `Candidate`. |

## Limitations

- Version 1 is Markdown-only and adds no scripts, CI, executable evals, or Go
  service code.

## Resume checkpoint

| Field | Value |
| --- | --- |
| Current unit | None; final Unit 2 prepared as `Done` |
| Plan position | `PLAN.md`, all units complete pending commit success |
| Last completed | Final verification and review passed; task prepared as `Completed` |
| Last verification | Final suite passed: policy matrix, routing/templates, local links, `git diff --check`, and scope check |
| Next action | Create the permitted final commit; statuses take effect only if it succeeds |
| Blocker | None |
| Uncommitted state | Archived final task record prepared for the permitted commit |
