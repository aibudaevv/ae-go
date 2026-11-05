# Task Workflow

This directory is the durable record for non-trivial repository work. The root
[`AGENTS.md`](../AGENTS.md) is the binding entry point. Existing planning rules,
approval boundaries, and the repository definition of done still apply.

## Classify the work

A request is small only when all of the following are true:

- It is local and unambiguous.
- It does not materially affect architecture, APIs, schemas, security, or
  operations.
- It does not need multiple independently verifiable implementation units.
- It has an obvious verification path.

Small standalone work uses neither a task directory nor units and does not
allocate a `task-NNN` identifier. Use a user-supplied identifier when present;
otherwise its commit subject omits one. If a small change belongs to a
non-trivial task, it is a unit of that task. Treat any uncertain classification
as non-trivial.

## Verify prerequisites

Codex is the reference runtime for this workflow. Before starting the first
non-trivial task, install the latest available releases from their canonical
sources:

- Install Superpowers from `https://github.com/obra/superpowers`. In Codex,
  open `/plugins`, find Superpowers, select `Install plugin`, and start a new
  chat. It supplies the `brainstorming` and `writing-plans` workflows.
- Use `$skill-installer` to install both `skills/productivity/grill-me` and
  `skills/productivity/grilling` from
  `https://github.com/mattpocock/skills`, then start a new Codex session.
  `grill-me` is the entry wrapper; this workflow invokes `$grilling`.

These dependencies intentionally track their latest releases rather than a
pinned version. Their discoverable capabilities are the compatibility gate.
After classifying work as non-trivial and before selecting an identifier,
confirm that the current session's skill catalog advertises all of:

- `superpowers:brainstorming`;
- `superpowers:writing-plans`;
- `grilling`.

If any capability is absent or cannot be loaded, stop and report its name with
the applicable installation instructions. Do not select a task identifier,
create a worktree or task artifacts, or substitute an ordinary self-review.
Small standalone work skips this preflight. Repeat it when resuming a task in
`Draft` or returning a task to `Draft` after a material change.

Another agent runtime may be used only when it exposes equivalent workflows
and can perform the same capability preflight; Codex is the only runtime with
installation steps verified by this template.

## Task, unit, and step

- A task is the user-visible outcome identified by the user-supplied ID or by
  a harness-allocated `task-NNN`.
- A unit is one independently reviewable result inside a task, limited to 300
  changed lines unless the repository planning rules document an exception.
- A step is one concrete `PLAN.md` action inside a unit.

Each unit normally maps to one verified and permitted commit.

## Allocate an identifier

Use the task identifier supplied by the user. If none was supplied, allocate a
`task-NNN`; generated identifiers begin with `task-001`. Task identifiers are
never reused.

Harness allocation is serialized: establish one allocation owner until the
directory is created or the attempt is abandoned.

Before using a user-supplied identifier, confirm it is not already used in
active task directories, `tasks/old/`, repository history, known refs, or
registered worktrees. Before allocating a generated identifier, inspect the
same sources, choose one greater than the highest `task-NNN`, then repeat the
check immediately before the first unit commit.

This lightweight harness cannot reserve an ID atomically across disconnected
clones. If later integration reveals an external collision, renumber the
unmerged task to the next free ID before integration. If known state is
ambiguous, stop for human confirmation rather than guessing.

Name the directory `<TASK-ID>-short-name` using the selected identifier and a
concise lowercase slug.
Cancelled, deleted, missing, active, and archived identifiers remain spent.

## Choose the workspace

After allocating the ID, ask for permission to create a worktree. If granted,
create it and perform all task work there. If refused, automatically continue
in the current checkout; the refusal authorizes that fallback and no second
workspace question is needed.

## Create and design the task

1. Create the task directory with copies of `templates/TASK.md` and
   `templates/GRILLING.md`; set status `Draft`.
2. Review open entries in [`IMPROVEMENTS.md`](IMPROVEMENTS.md) whose area or
   trigger matches the proposed scope, then create the first `DESIGN.md` draft
   through the brainstorming workflow. Do not scan archived tasks for promoted
   knowledge; its canonical owner is authoritative.
3. Invoke `$grilling`, record its questions and decisions in `GRILLING.md`, and
   revise the design. Do not substitute ordinary self-review or another skill.
   For every relevant improvement, record whether the design promotes, rejects,
   or defers it and why.
4. Stop while any material question remains unresolved.
5. Obtain human approval of the current `DESIGN.md`. A material change after
   approval invalidates it and requires renewed approval.
6. Create `PLAN.md` through the writing-plans workflow.
7. Set status `Ready` only when all four files exist, the design is approved,
   material grilling questions are resolved, and the plan is actionable.

There is no separate planning commit. The first unit commit also introduces
the initial task artifacts.

## Artifact authority

- `DESIGN.md` owns intent, requirements, scope, architecture decisions, and
  acceptance criteria identified as `AC-1`, `AC-2`, and so on.
- `GRILLING.md` owns challenges, assumptions, alternatives, risks, decisions,
  and unresolved questions. Preserve history; append or explicitly supersede
  decisions instead of silently rewriting them.
- `PLAN.md` owns units, files, interfaces, TDD steps, verification commands,
  and expected results.
- `TASK.md` is a compact manifest of task status, artifact links, unit status,
  acceptance results, observed evidence, the compounding review, and the resume
  checkpoint.
- `IMPROVEMENTS.md` owns deferred reusable signals, their independent evidence
  sources, promotion status, proposed canonical target, and next decision.
- `.superpowers/sdd/.../progress.md` is a temporary detailed execution ledger.

Do not copy detailed scope, implementation instructions, planned checks, or
the full step-by-step history into `TASK.md`. Keep only the current `PLAN.md`
position and enough observed state to resume safely. `TASK.md` also does not
store commit hashes: a commit cannot contain its own stable hash.

## Compound engineering loop

Before a non-trivial task becomes `Completed`, review evidence produced during
its work, including user corrections, invalidated assumptions, review findings,
failed checks, repeated manual steps, and successful reusable patterns. Record
each material signal in the task's `Compounding review` with one outcome:

- `No action`: the evidence is task-specific or contains no reusable learning;
  state the reason.
- `Promoted`: the approved task updated the single canonical owner; link it.
- `Deferred`: create or update an entry in `IMPROVEMENTS.md` and link its ID.

An absent compounding review blocks completion. The review may contain a single
reasoned `No action`; do not invent a permanent rule merely to close a task.
Small standalone work remains exempt because it creates no task artifacts.

### Register and readiness

Give a new deferred signal the ID `<source-task-id>-I<N>`, using the next
sequence number within its source task. Before adding it, look for an existing
entry with the same underlying trigger and proposed owner. Update that entry's
evidence sources instead of creating a duplicate, and preserve every distinct
source task ID. Multiple observations within one task remain one independent
source.

Improvement statuses are `Candidate`, `Ready`, `Promoted`, and `Rejected`:

- `Candidate`: an ordinary signal has one independent task source.
- `Ready`: the signal is a serious risk or has at least two independent task
  sources.
- `Promoted`: the canonical owner was updated; link that target in the decision.
- `Rejected`: evidence showed that the signal should not become repository
  guidance; preserve the rationale.

A serious risk is a security or privacy exposure, data loss or corruption, a
public-contract break, an escaped defect, or inability to run a mandatory
check. It becomes `Ready` from one source. Ordinary failure-derived signals and
successful reusable patterns both require two independent task sources.

The readiness threshold applies to every promotion, including a direct
task-local promotion that has no existing register entry. A first ordinary
signal remains `Candidate`; approved scope is necessary for promotion but does
not replace the evidence threshold.

When a design encounters a relevant `Ready` entry, it must choose promotion,
rejection, or reasoned deferral. An out-of-scope entry does not block the task,
but `GRILLING.md` must state why it remains out of scope and the next trigger
that requires reconsideration. Every later relevant design makes the choice
again; it does not rely on an earlier deferral.

### Promotion ownership

Promote always-loaded policy and routing into `AGENTS.md`, topic-specific
engineering knowledge into its matching `docs/` owner, artifact shape into
`tasks/templates/`, and lifecycle mechanics into this file. A cross-link may
aid discovery, but the rule itself has one canonical owner. Potential scripts,
skills, CI, and executable evals remain deferred candidates until a separately
approved task implements them.

Promotion in the current task is allowed only when its approved `DESIGN.md`
includes that change. Otherwise record a deferred follow-up; never expand scope
silently. Register entries must not contain secrets, sensitive personal data,
or unnecessary incident details.

## Maintain the resume checkpoint

Initialize the checkpoint when creating the task. Update it when starting a
unit and after a meaningful transition such as an expected failing test, a
completed implementation, a passing verification, a migration or generation,
or a decision that changes the next action. Also update it on an error or
blocker, before pausing, handing off, or ending a session, before requesting a
commit, and after a successful commit when selecting the next unit.

Record the current unit, exact `PLAN.md` position, last completed result, last
observed verification, single next action, blocker, and relevant uncommitted
state. Do not update the checkpoint after routine file reads, searches,
formatting, or other actions that do not change how work should resume.

Before a unit commit the checkpoint is local working-tree state and supports a
restart in the same workspace. The unit commit makes the checkpoint durable in
Git. Do not claim that an uncommitted checkpoint survives loss of the
workspace.

## Lifecycle

The task lifecycle is:

```text
Draft -> Ready -> In Progress -> Completed -> tasks/old/
                         \-> Blocked -> In Progress
```

- `Draft`: design, grilling, or planning is incomplete.
- `Ready`: all four artifacts exist and implementation may start.
- `In Progress`: implementation or verification is underway.
- `Blocked`: progress requires permission, human input, or external change.
- `Completed`: the final permitted commit succeeded after final review.
- `Cancelled`: work stopped permanently; preserve its ID and rationale under
  `tasks/` because `tasks/old/` is reserved for completed work.

Unit statuses are `Pending`, `In Progress`, `Blocked`, and `Done`. A status of
`Done` becomes effective only after the unit's permitted commit succeeds.

## Execute a unit

For each unit:

1. Set the task and unit to `In Progress` and point the resume checkpoint to
   the unit's first `PLAN.md` step.
2. Follow the unit's `PLAN.md` steps and applicable engineering rules.
3. Run every required verification and observe its result.
   If a required verification is unavailable, keep the task and unit `Blocked`
   until the user explicitly accepts the remaining risk. Record the check,
   reason, and acceptance in `TASK.md`.
4. Update `TASK.md` with unit-level evidence and a current resume checkpoint,
   leaving the unit `In Progress`.
5. Show the current unit diff and ask for explicit permission to commit it.
   A material change to that diff invalidates the permission and requires
   renewed permission.
6. If permission is refused or absent, mark the task and unit `Blocked` and do
   not begin another unit.
7. If granted, prepare `Done` and create one commit with trailers:

   ```text
   Task: <TASK-ID>
   Unit: N
   ```

8. Treat `Done` as effective only after commit success. Then point the
   checkpoint to the next unit. On failure, retain or restore a resumable
   `Blocked` state and keep the ledger.

Permission for a unit commit does not authorize push, merge, PR creation,
deployment, production changes, or another external write.

## Handle changes

A local technical correction that preserves approved observable behavior may
be recorded as a ledger ruling and executed without reopening the design.

A material scope, acceptance-criteria, API, or architecture change returns the
task to `Draft`. Update `DESIGN.md`, repeat `$grilling`, obtain approval, and
rebuild `PLAN.md` before resuming implementation.

## Complete and archive

Run final verification and whole-task review before the final unit commit,
including the uncommitted final unit. Resolve all blocking findings first.

While the task remains `In Progress`:

1. Complete the compounding review and resolve each material signal as
   `No action`, `Promoted`, or `Deferred`.
2. Update acceptance results and observed evidence in `TASK.md`.
3. Ask permission for the final unit commit.
4. If refused or absent, mark the task and unit `Blocked`, keep the active
   directory and ledger, and stop.
5. If granted, prepare the unit as `Done`, the task as `Completed`, and move the
   entire directory unchanged to `tasks/old/`.
6. Create the final unit commit with the required task and unit trailers.
7. Treat completion and archival as effective only after commit success; on
   failure, retain or restore the active `Blocked` state.
8. Before deleting the ledger, transfer any decision needed to understand the
   final implementation or its limitations to the authoritative task artifact.
9. Delete the temporary execution ledger only after commit success.

Closure does not require a separate unit. For a one-unit task, its only commit
may introduce the complete record directly under `tasks/old/`.

## Artifact ownership after archival

The archived directory is the immutable durable task record. Keep internal
links relative so they survive the move. Correct later factual mistakes
explicitly rather than silently rewriting execution history.
