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
allocate a task identifier. Use a user-supplied identifier when present;
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
  a harness-generated ID.
- A unit is one independently reviewable result inside a task, limited to 300
  changed lines unless the repository planning rules document an exception.
- A step is one concrete plan action inside a unit.

Each unit normally maps to one verified and permitted commit. Verified units
may remain uncommitted while later units proceed; keep their boundaries clear.

## Allocate an identifier

Use the user-supplied ID, or generate `task-YYYYMMDD-<8 random hex digits>`
using the current UTC date, for example `task-20261105-148ae712`. Check active
and archived task directory names for the exact ID; regenerate on a generated
collision. An existing task ID means resume that task when intended; otherwise
ask for a distinct user-supplied ID. Never deliberately reuse an ID.

Name the directory `<TASK-ID>-short-name` with a concise lowercase slug.
No sequential counter, history scan, ref scan, or cross-clone reservation is
required. Resolve any collision discovered during integration before merging.
Existing identifiers, including historical `task-NNN` IDs, remain valid.

## Choose the workspace

Honor an existing workspace choice in the session. Otherwise ask for permission
to create a worktree. If granted, work there; if refused, continue in the current
checkout without asking again.

## Create and design the task

1. Create the task directory with a copy of `templates/TASK.md`; set `Draft`.
2. Review open entries in [`IMPROVEMENTS.md`](IMPROVEMENTS.md) whose area or
   trigger matches the scope, then draft the `Design` section through the
   brainstorming workflow. Promoted knowledge lives in its canonical owner;
   do not scan archived tasks to recover it.
3. Invoke `$grilling`, record material questions and decisions in the task,
   and revise the design. Do not substitute another workflow. Record the
   disposition of relevant improvements without repeating their evidence.
4. Stop while any material question remains unresolved.
5. Obtain design approval. Approval of a concrete proposal already given in the
   session counts; record it rather than asking again. Material changes require
   renewed approval.
6. Write the `Plan` through the writing-plans workflow, following the repository
   planning rules. Keep commands, expected results, and dependencies actionable.
7. Set `Ready` when the design is approved, material questions are resolved, and
   the plan is actionable.

The first unit commit includes the task record; no planning-only commit is
required.

## Artifact authority

`TASK.md` is the default single record: design and acceptance criteria,
questions and decisions, implementation plan, verification evidence,
compounding review, and resume checkpoint. Preserve material decisions;
explicitly supersede them when they change.

Extract a large section into `DESIGN.md`, `GRILLING.md`, or `PLAN.md` only when
that improves readability. Link the extracted section from `TASK.md` and keep
one owner for each fact. `templates/GRILLING.md` is an optional extraction aid.
Existing four-file tasks remain valid and need no migration.

`IMPROVEMENTS.md` owns unresolved reusable signals and their disposition. A
separate execution ledger is optional; if a tool needs one, keep it temporary
and copy durable decisions and evidence into the task before handoff or cleanup.
Do not duplicate step-by-step progress across the plan, checkpoint, and ledger.

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

### Improvement register

Give a new deferred signal the ID `<source-task-id>-I<N>`, using the next
sequence number within its source task. Before adding it, look for an existing
entry with the same underlying trigger and proposed owner. Update that entry's
evidence sources instead of creating a duplicate, and preserve every distinct
source task ID. Multiple observations within one task remain one independent
source.

Each entry records its ID, status, area or trigger, evidence links, proposed
canonical target, and decision or next action. Status is `Open`, `Applied`, or
`Rejected`: unresolved signals stay open; applied entries link the updated
owner; rejected entries retain the rationale. Preserve IDs and evidence when
updating an entry. Do not rewrite archived task records to migrate old statuses.

Two independent task sources are useful evidence for a general rule, not a
promotion prerequisite. A verified correction or well-supported improvement
may be applied from one source within approved scope. Prioritize security,
privacy, data integrity, public-contract failures, escaped defects, and blocked
mandatory checks without waiting for repetition. Do not generalize a local
workaround into permanent policy without a rationale.

During design, record the disposition of relevant open entries: apply, reject,
or defer with a reason and a concrete next trigger. Out-of-scope entries do not
block the task. Reconsider them when that trigger or a relevant design occurs.

### Promotion ownership

Promote always-loaded policy and routing into `AGENTS.md`, topic-specific
engineering knowledge into its matching `docs/` owner, artifact shape into
`tasks/templates/`, and lifecycle mechanics into this file. A cross-link may
aid discovery, but the rule itself has one canonical owner. Potential scripts,
skills, CI, and executable evals remain deferred candidates until a separately
approved task implements them.

Promotion in the current task is allowed only when its approved design
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

Record the current unit, exact plan position, last completed result, last
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
- `Ready`: design is approved, questions resolved, and the plan actionable.
- `In Progress`: implementation or verification is underway.
- `Blocked`: progress requires permission, human input, or external change.
- `Completed`: the final permitted commit succeeded after final review.
- `Cancelled`: work stopped permanently; preserve its ID and rationale under
  `tasks/` because `tasks/old/` is reserved for completed work.

Unit statuses are `Pending`, `In Progress`, `Blocked`, `Verified`, and `Done`.
`Verified` means checks passed and the diff was reviewed; later units may start.
`Done` becomes effective only after the unit's permitted commit succeeds.

## Local commit authorization

The user may authorize all local unit commits within an approved task scope
once, or authorize commits individually. Record the permission and its scope
in the task; honor permission already given in the session without asking again.
Design approval alone is not commit authorization. A material change beyond
the authorized scope requires renewed permission.
Individual permission covers the reviewed unit diff; a material change to that
diff requires renewed permission. Task-wide permission covers units within the
approved task scope.

Without commit permission, keep verified work uncommitted and continue other
planned units. At handoff, show the reviewable result and request permission
only if committing is the remaining required action. An explicit instruction
to stop or refusal of further work must be honored.

Local commit permission does not authorize push, merge, PR creation, deployment,
production changes, or another external write.

## Execute a unit

For each unit:

1. Set the task and unit to `In Progress`; update the checkpoint to the unit.
2. Follow its plan and applicable engineering rules.
3. Run every required verification and observe its result.
   If a required verification is unavailable, keep the task and unit `Blocked`
   until the user explicitly accepts the remaining risk. Record the check,
   reason, and acceptance in `TASK.md`.
4. Review the unit diff, record evidence, and set `Verified`.
5. If local commit permission covers the unit, prepare `Done` and create one
   commit with trailers; otherwise retain `Verified` and continue:

   ```text
   Task: <TASK-ID>
   Unit: N
   ```

6. After commit success, `Done` is effective. On failure, restore `Verified`,
   record the error and next action, and never report a successful commit.
   Keep the task `Blocked` if that failure prevents further work.
7. Update the checkpoint before continuing to the next unit.

When committing several previously verified units, preserve their planned
boundaries, include each unit's required documentation and tests, and review
the staged diff. Do not sweep later units into an earlier commit.

## Handle changes

A local technical correction that preserves approved observable behavior may
be recorded in the task's decisions and executed without reopening the design.

A material scope, acceptance-criteria, API, or architecture change returns the
task to `Draft`. Update the design, repeat `$grilling` for the changed decisions,
obtain approval, and revise the plan before resuming implementation.

## Complete and archive

Run final verification and whole-task review before the final unit commit,
including the uncommitted final unit. Resolve all blocking findings first.

While the task remains `In Progress`:

1. Complete the compounding review and resolve each material signal as
   `No action`, `Promoted`, or `Deferred`.
2. Update acceptance results and observed evidence in `TASK.md`.
3. Transfer durable decisions and evidence from any temporary tool ledger.
4. Commit remaining verified units only when authorized. Task-wide permission
   needs no repeated confirmation; otherwise request the missing permission
   after the complete result is ready for review.
5. While permission is absent, keep the task active with verified units and a
   checkpoint stating that commits remain. Do not claim completion or archive.
6. For the final authorized commit, prepare the final unit as `Done`, the task
   as `Completed`, and move the whole directory to `tasks/old/`.
7. Create that commit with the required trailers. Completion and archival take
   effect only on success; on failure restore the active directory and actual
   unit states, and record the blocker.
8. Delete any temporary tool ledger only after commit success and evidence
   transfer. The durable task record is sufficient without a separate ledger.

Closure does not require a separate unit. For a one-unit task, its only commit
may introduce the complete record directly under `tasks/old/`.

## Artifact ownership after archival

The archived directory is the immutable durable task record. Keep internal
links relative so they survive the move. Correct later factual mistakes
explicitly rather than silently rewriting execution history.
