# task-20261105-b7a005fa: Make the harness lighter

Status: Verified
Mode: Full design; six proposals approved in the preceding review.
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
| 4 | Compact Go Style while preserving local conventions | 200–290 | Compare every original rule with shortened guidance or baseline reference | Verified |
| 5 | Compact commit guidance | 150–240 | Subject, ID, breaking change, permission, trailers, squash rules | Verified |
| 6 | Compact planning guidance | 120–220 | Cohesion, size accounting, generated output, exception and plan requirements | Verified |
| 7 | Whole-task consistency, final evidence and closure | 180–290 including archive move | Local links/anchors, stale policy search, scenario review, diff check | Verified |

Run focused policy and diff review after each unit; run the complete document
checks once at final handoff. Fixes trigger the affected checks again.

## Verification

Baseline: no Go module or service source; Go build/test/module/lint checks are
not applicable. `git status --short` was empty before editing.

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
- Unit 4: Go Style retains every normative rule, including declaration groups,
  blank-line conventions, naming, consumer-owned interfaces, contexts, error
  wrapping, and resource ownership. Removed teaching examples and the duplicate
  checklist; background stays available in the existing baseline links.
  Compared the shortened page with the original; whitespace check passed.
- Unit 5: commit guidance retains subject grammar, ID rules, types, scopes,
  bodies, trailers, breaking-change semantics, coherent boundaries, squash
  behavior, and explicit permission. References point to the authoritative
  authorization and unit-trailer rules. Diff and whitespace review passed.
- Unit 6: planning retains the 300-line limit, actual size checks, cohesive
  behavioral slices, tests with their behavior, dependency contracts, separate
  generated/mechanical accounting, and all exception requirements. Removed
  repeated explanations and the duplicate review checklist. Diff and whitespace
  review passed. Unit 7 may include concise workflow reconciliation and a
  mechanical archive move; hand-written changes stay below 300 lines.
- Unit 7: whole-task review and an independent read-only reviewer found no
  actionable findings. Before closure, document validation covered 22 Markdown
  files, 31 local links, and 7 anchors with no failures; fences were balanced.
  Twelve protected documents and the engineering rules, definition of done,
  and final-review requirements in AGENTS.md are unchanged. Archive-aware link,
  unit-size, and whitespace checks are repeated at handoff.
- Unit snapshot diffs 1–6: 233, 110, 73, 135, 179, and 179 changed lines.
  Unit 7 includes the mechanical move of this record into `tasks/old/`,
  checked separately from policy edits; no generated output exists.
- Existing active guidance: 1579 -> 1284 lines (18.7% fewer), excluding this
  task record and historical tasks. This measures document size, not runtime
  speed or agent outcome quality; no performance benchmark was run.

Scenario walkthroughs against the final policy (not executed agent evals):

| Scenario | Result |
| --- | --- |
| Read-only review | Findings in the response, no task artifacts |
| Small change with or without a supplied ID | No generated ID or task record; supplied ID retained |
| Clear ordinary work across units | Compact record and plan; no extra design gate |
| Material API, authorization, or migration change | Full design, risk review, material questions resolved, explicit design approval |
| Missing or incompatible skill | Required results prepared directly; no installation gate |
| Verified work without commit permission | Can close and archive; uncommitted state explicit; no commit |
| Commit fails or hook changes files | Failure reported separately; changed tested files reverified |
| Required check unavailable | Blocked until explicit risk acceptance; inapplicable checks explained |
| Handoff or interruption | Four-field checkpoint; necessary intermediate state retained |
| Go/module change or documentation-only change | Full affected-module/race checks for Go; document checks for prose; rerun after affected changes |

## Compounding review

- Promoted: the approved six simplifications are owned by the
  [workflow](../../README.md), [agent routing](../../../AGENTS.md),
  [development guidance](../../../docs/development/planning.md), and
  [task template](../../templates/TASK.md). Evidence is the approved review,
  retained-rule comparison, scenario walkthroughs, and independent review.
- No action: no additional reusable defect emerged during implementation.
  Existing `task-001-I1` stays open for actual external-executor use; changing
  dependency policy does not prove that executor's compatibility.

## Resume checkpoint

- Position: All seven units verified; closed under the new workflow.
- Verified: Policy scenarios, independent review, document and diff checks.
- Next action: None for implementation; local commits remain unauthorized.
- Blocker: None.

## Publication authorization addendum

After archival, the user explicitly instructed "нет, коммить и пушь".
This authorizes committing the seven reviewed units and pushing the current
`master` branch to `origin/master`. Earlier authorization statements describe
the original uncommitted handoff. Observe and report publication results from
Git; this authorization does not imply that either operation has succeeded.
