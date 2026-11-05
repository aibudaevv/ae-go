# task-20261105-b7a005fa: Make the harness lighter

Status: In Progress
Created: 2026-11-05
Workspace: Current checkout; documentation changes only.
Git state: Uncommitted. Local commits are not authorized.

## Design

Implement the six review proposals approved by the user in this session:
adaptive task routing, optional skills, completion independent of commits,
smaller checkpoints, shorter frequently read guidance, and explicit check
scheduling. Keep review units at 300 changed lines, regression tests, Mc/DC,
security rules, approval boundaries, and the compounding learning loop.
No service code, dependencies, CI, or permanent scripts are introduced.

Acceptance: routing distinguishes read-only work, ordinary changes, and work
requiring full design; missing skills do not block work; verified tasks can
close uncommitted; checkpoints have four fields; local conventions survive
compression; final applicable verification remains mandatory.

## Questions and decisions

- The user approved all six concrete proposals; design approval is recorded
  here rather than requested again. No material question remains.
- Use the current checkout and execute inline. The working tree was clean.
- The current catalog exposes brainstorming, writing-plans, and grilling.
  The approved change removes their mandatory role; no new interview is needed
  for decisions already settled in the review.
- Relevant improvement `task-001-I1` concerns an external executor. Keep it
  open: this task makes skills optional but does not repair that executor.
- Preserve archived task history. Explain compatibility with old statuses.

## Plan

All units are documentation-only, with no generated output. Each produces a
coherent policy change and includes its routing or template updates. Units
2 and 3 build on unit 1; units 4–6 preserve independent topic conventions;
unit 7 reconciles planning with the completed workflow.

| Unit | Purpose and files | Estimated changed lines | Verification | Status |
| --- | --- | --- | --- | --- |
| 1 | Adaptive process and optional skills: workflow, AGENTS, README, templates, this record | 240–300 | Routing and missing-skill scenarios; links; diff | Verified |
| 2 | Independent work/Git states: workflow and task template | 170–260 | Uncommitted completion, authorization, commit failure, archive compatibility | Verified |
| 3 | Four-field checkpoints and check scheduling: workflow, AGENTS, task template | 100–190 | Resume, unavailable checks, documentation and Go verification paths | Verified |
| 4 | Compact Go Style while preserving local conventions | 200–290 | Compare every original rule with shortened guidance or baseline reference | Pending |
| 5 | Compact commit guidance | 150–240 | Subject, ID, breaking change, permission, trailers, squash rules | Pending |
| 6 | Compact planning guidance | 120–220 | Cohesion, size accounting, generated output, exception and plan requirements | Pending |
| 7 | Whole-task consistency, final evidence and closure | 40–140 | Local links/anchors, stale policy search, scenario review, diff check | Pending |

Run focused policy and diff review after each unit; run the complete document
checks once at final handoff. Fixes trigger the affected checks again.

## Verification

Baseline: no Go module or service source; Go build/test/module/lint checks are
not applicable. `git status --short` was empty before editing. Final results
and actual unit sizes will be recorded here.

- Unit 1: routing review covers read-only, small, ordinary, and full-design
  changes. Missing skills now allow direct preparation; material questions and
  approval still gate full design. `git diff --check` passed.
- Unit 2: an unchanged verified diff remains verified without a commit or after
  commit failure; hooks changing files require reverification. Commit approval
  remains explicit, Git state is reported separately, and historical statuses
  remain valid. Whitespace and lifecycle-reference review passed.
- Unit 3: checkpoint policy now covers unit boundaries, blockers, and handoff
  using four fields. Check scheduling distinguishes focused iteration, full
  affected-module verification, and documentation-only checks. Required but
  unavailable checks remain blocked pending explicit risk acceptance;
  inapplicable checks are explained. Policy and whitespace review passed.

## Compounding review

Pending: review observed signals before closing the task.

## Resume checkpoint

- Position: Unit 3 verified; Unit 4 next.
- Verified: Checkpoint and verification scheduling scenarios; whitespace passed.
- Next action: Shorten Go Style while preserving local conventions.
- Blocker: None.
