# Compounding Learning Loop Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use
> `superpowers:executing-plans` to implement this plan unit-by-unit. Steps use
> checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a Markdown-only learning loop that turns evidence from completed
non-trivial tasks into canonical guidance or traceable improvement candidates.

**Architecture:** `tasks/README.md` owns the lifecycle and
`tasks/IMPROVEMENTS.md` owns deferred candidates. The root instructions and
templates expose only the triggers and fields needed to enter that lifecycle,
preserving one canonical owner for every rule.

**Tech Stack:** Markdown, Git, shell-based static checks.

**Spec:** `DESIGN.md`

## Global constraints

- Version 1 adds no scripts, CI, executable evals, repo-local skills,
  dependencies, or Go service code.
- A task review may conclude `No action`; permanent improvement is never
  invented to satisfy the process.
- Readiness is `serious risk OR at least two independent task sources`.
- Promotion changes one canonical owner and never silently expands approved
  scope.
- Unit commits require explicit user permission and the task trailers defined
  by `tasks/README.md`.

## Review focus

- One ordinary source must remain `Candidate`, while one serious source must
  become `Ready`.
- Two observations inside the same task must not count as two independent
  sources.
- A relevant out-of-scope `Ready` item must remain visible without blocking an
  unrelated task or silently expanding it.
- `No action`, `Promoted`, and `Deferred` must point to evidence, a canonical
  target, or a register ID as applicable.
- The root instructions must route into the lifecycle without duplicating the
  detailed policy.

---

### Task 1 (Unit 1): Define the canonical learning lifecycle

**Purpose:** Establish the deferred-improvement contract and all decisions that
govern it.

**Estimated size:** 520–560 hand-written changed lines, including the four
mandatory initial task artifacts.

**Size exception:** The task artifacts already contain 248 lines before
`PLAN.md`. The repository forbids a separate planning commit and requires the
first unit commit to introduce the complete task record. Splitting the artifacts
would commit a task that cannot reach `Ready`; separating policy from the
register would leave the lifecycle without its required state owner. The
considered alternatives were a forbidden planning-only commit, partial task
artifacts, and combining both implementation units into a still larger commit.
Reviewers should verify artifact authority, registry state transitions, and the
Mc/DC scenarios below; the primary risk is duplicated or contradictory policy.

**Inputs/dependencies:** Approved `DESIGN.md` and resolved `GRILLING.md`.

**Produces:** The canonical lifecycle in `tasks/README.md` and the deferred
candidate interface in `tasks/IMPROVEMENTS.md`.

**Generated/mechanical changes:** None.

**Files:**

- Modify: `tasks/README.md`
- Create: `tasks/IMPROVEMENTS.md`
- Include: `tasks/task-001-compounding-loop/{TASK,DESIGN,GRILLING,PLAN}.md`

- [ ] **Step 1: Record the expected failing baseline**

Run searches proving that `tasks/IMPROVEMENTS.md`, compounding review rules,
promotion thresholds, and relevant-design review do not yet exist. Expected:
the file check and required-term searches fail.

- [ ] **Step 2: Create the improvement register contract**

Define ID format `<source-task-id>-I<N>`, fields, statuses `Candidate`, `Ready`,
`Promoted`, and `Rejected`, append/update rules, source independence, sensitive
data exclusion, and an initially empty register table.

- [ ] **Step 3: Extend the canonical task lifecycle**

Update artifact authority, design/grilling, completion, and archive rules in
`tasks/README.md`. Specify the three task-local outcomes, the readiness
decision, canonical promotion targets, relevant `Ready` deferral, deduplication,
and the prohibition on silent scope growth.

- [ ] **Step 4: Verify the policy decision matrix**

Inspect these Mc/DC cases against the written rules:

| Serious risk | Independent sources | Expected status |
| --- | ---: | --- |
| No | 1 | `Candidate` |
| Yes | 1 | `Ready` |
| No | 2 | `Ready` |

Also verify that two observations from one task remain one source, positive
patterns need two sources, and a reasoned `No action` permits completion.

- [ ] **Step 5: Verify Unit 1 and prepare its checkpoint**

Run required-term searches, local-link checks, `git diff --check`, and inspect
the Unit 1 diff for conflicting artifact authority. Update `TASK.md` with
observed evidence and leave the unit `In Progress` until commit permission.

- [ ] **Step 6: Request permission and commit Unit 1**

After showing the complete Unit 1 diff, request explicit permission. If
granted, commit with subject `docs: task-001 define compounding improvement
lifecycle` and trailers `Task: task-001` and `Unit: 1`. If refused or absent,
mark the task and unit `Blocked` and stop.

---

### Task 2 (Unit 2): Route agents through the learning loop

**Purpose:** Make the canonical lifecycle discoverable and mandatory at the
correct task phases.

**Estimated size:** 60–110 hand-written changed lines.

**Inputs/dependencies:** Unit 1 register and lifecycle contract.

**Produces:** Root routing, template fields, and repository orientation for the
compounding loop.

**Generated/mechanical changes:** None.

**Files:**

- Modify: `AGENTS.md`
- Modify: `README.md`
- Modify: `tasks/templates/TASK.md`
- Modify: `tasks/templates/GRILLING.md`

- [ ] **Step 1: Record the expected failing routing baseline**

Search for `tasks/IMPROVEMENTS.md`, `Compounding review`, relevant accumulated
knowledge, and the three outcomes in the target files. Expected: required terms
are absent before the edits.

- [ ] **Step 2: Add concise root routing and orientation**

Require relevant-candidate review during design and compounding review before
completion in `AGENTS.md`; keep details in `tasks/README.md`. Describe the loop
briefly in `README.md` without presenting a second policy.

- [ ] **Step 3: Add template evidence fields**

Add relevant-improvement disposition to `GRILLING.md` and a compact compounding
review table to `TASK.md`. Preserve each artifact's existing authority.

- [ ] **Step 4: Run final verification and review**

Verify all local Markdown links, required routing terms, lifecycle consistency,
scenario outcomes, `git diff --check`, and final diff scope. Go build, test,
race, module, and lint commands are not applicable because the repository has
no Go module or source.

- [ ] **Step 5: Close the task and prepare the final checkpoint**

Record acceptance evidence and a compounding review for this task. If no new
signal emerged beyond the implemented design, record reasoned `No action`.
Keep Unit 2 and the task `In Progress` until commit permission is granted.

- [ ] **Step 6: Request permission and commit Unit 2**

After showing the complete final-unit diff, request explicit permission. If
granted, prepare Unit 2 as `Done`, set the task to `Completed`, move the complete
directory to `tasks/old/`, and commit with subject `docs: task-001 route tasks
through compounding review` and trailers `Task: task-001` and `Unit: 2`. On
refusal or failure, retain or restore a resumable `Blocked` state and do not
archive as completed.
