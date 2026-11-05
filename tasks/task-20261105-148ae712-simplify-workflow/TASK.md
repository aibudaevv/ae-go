# task-20261105-148ae712: Simplify the engineering workflow

Status: In Progress
Created: 2026-11-05
Workspace: Current checkout, as requested by the user.

## Design

Implement the six simplifications approved in the review: one task document
by default, task-wide local commit authorization, advisory evidence thresholds,
three improvement statuses, concise agent routing, and date/random task IDs.
Keep verification, review-unit sizing, design approval, scope control, and
separate authorization for external writes. Add no code, dependencies, or CI.

## Questions and decisions

- The user approved all six review proposals; no material question remains.
- Use inline design, questions, plan, evidence, and checkpoint sections here.
  Separate files remain optional for large sections; archived tasks stay intact.
- Allow further verified units before commit authorization, without committing.
  Task-wide commit authorization must be explicit; approving a design alone
  does not grant it. The user subsequently authorized all reviewed local unit
  commits with "коммить"; this does not authorize external writes.
- Generated IDs use a UTC date and eight random hex digits. Check current task
  directories for collisions; preserve historical IDs without renumbering.
- `task-001-I1` concerns external executor compatibility, beyond these six
  changes. Keep it open; reconsider when that executor is next used, without
  requiring another failure first.

## Plan

Each unit is documentation-only, has no generated changes, and must pass diff
review, whitespace checks, and applicable routing/link checks.

| Unit | Purpose and files | Estimate | Verification | Status |
| --- | --- | --- | --- | --- |
| 1 | Simplify learning rules and register in tasks/README.md and IMPROVEMENTS.md; include this record | 200–280 lines | One-source corrections allowed; statuses and next trigger consistent | Done |
| 2 | Replace sequential IDs and duplicate routing in tasks/README.md, AGENTS.md, commits.md | 100–180 lines | No active task-NNN allocation rules; routing remains authoritative | Verified |
| 3 | Consolidate artifacts in workflow, templates, root README, architecture guidance | 200–290 lines | Single-file and split-file tasks both supported; local links resolve | Verified |
| 4 | Decouple verification from commits in workflow and template | 120–220 lines | Task-wide permission, no-permission continuation, failure and archival cases | Verified |

Units 2–4 build on the existing workflow; unit 4 uses unit 3's inline plan.
Inspect each unit's diff independently before continuing. Finish with a
whole-change consistency review and local Markdown link validation.

## Verification

- All four unit diffs reviewed; whitespace checks passed during implementation.
- Local Markdown validation: 21 files, 26 links and anchors, zero failures.
- Scenario review: single-file and extracted-file tasks both have one owner per
  fact; historical IDs stay valid; generated ID collisions trigger regeneration.
- Authorization review: task-wide permission avoids repeated prompts;
  individual permission covers its reviewed diff; design approval grants no
  commit permission; uncommitted verified units may proceed; failed commits
  restore actual state; completion and archival still require commit success.
- Improvement review: a one-source correction may be applied in scope;
  unsupported general rules remain open; out-of-scope work stays deferred.
- Independent read-only review: no actionable findings; archived tasks intact.
  Follow-up clarified individual diff permission versus task-wide scope.
- Go build/test/module/lint checks are not applicable: no Go module or source.

## Compounding review

- Promoted: the six approved process simplifications now live in the canonical
  task workflow, task template, agent routing, and contextual documentation.
  Evidence: review approval and the 433-line record for the prior task.
- Deferred: existing `task-001-I1` stays open for the next executor use; its
  evidence link is preserved and its trigger no longer requires another failure.

## Resume checkpoint

All four units verified; implementation and review finished. The user authorized
the local commits. Unit 1 is prepared as Done, effective on commit success.
Next action: commit Unit 1, then the remaining reviewed units in order.
