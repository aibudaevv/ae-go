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

Small standalone work uses neither a task directory nor units. If a small
change belongs to a non-trivial task, it is a unit of that task. Treat any
uncertain classification as non-trivial.

## Task, unit, and step

- A task is the user-visible outcome identified by `task-NNN`.
- A unit is one independently reviewable result inside a task, limited to 300
  changed lines unless the repository planning rules document an exception.
- A step is one concrete `PLAN.md` action inside a unit.

Each unit normally maps to one verified and permitted commit.

## Allocate an identifier

Identifiers begin with `task-001` and are never reused. Allocation is
serialized: establish one allocation owner until the directory is created or
the attempt is abandoned.

Before allocation, inspect active task directories, `tasks/old/`, repository history,
all known local and remote refs, and registered worktrees in the current clone.
Choose one greater than the highest identifier found, then repeat the check
immediately before the first unit commit.

This lightweight harness cannot reserve an ID atomically across disconnected
clones. If later integration reveals an external collision, renumber the
unmerged task to the next free ID before integration. If known state is
ambiguous, stop for human confirmation rather than guessing.

Name the directory `task-NNN-short-name` using a concise lowercase slug.
Cancelled, deleted, missing, active, and archived identifiers remain spent.

## Choose the workspace

After allocating the ID, ask for permission to create a worktree. If granted,
create it and perform all task work there. If refused, automatically continue
in the current checkout; the refusal authorizes that fallback and no second
workspace question is needed.

## Create and design the task

1. Create the task directory with copies of `templates/TASK.md` and
   `templates/GRILLING.md`; set status `Draft`.
2. Create the first `DESIGN.md` draft through the brainstorming workflow.
3. Invoke `$grilling`, record its questions and decisions in `GRILLING.md`, and
   revise the design. Do not substitute ordinary self-review or another skill.
4. Stop while any material question remains unresolved.
5. Obtain human approval of `DESIGN.md`.
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
  acceptance results, observed evidence, blockers, and the next action.
- `.superpowers/sdd/.../progress.md` is a temporary detailed execution ledger.

Do not copy detailed scope, implementation instructions, planned checks, or
step-level progress into `TASK.md`. It also does not store commit hashes: a
commit cannot contain its own stable hash.

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

1. Set the task and unit to `In Progress`.
2. Follow the unit's `PLAN.md` steps and applicable engineering rules.
3. Run every required verification and observe its result.
4. Update `TASK.md` with unit-level evidence and the next action, leaving the
   unit `In Progress`.
5. Ask for explicit permission to commit the unit.
6. If permission is refused or absent, mark the task and unit `Blocked` and do
   not begin another unit.
7. If granted, prepare `Done` and create one commit with trailers:

   ```text
   Task: task-NNN
   Unit: N
   ```

8. Treat `Done` as effective only after commit success. On failure, retain or
   restore a resumable `Blocked` state and keep the ledger.

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

1. Update acceptance results and observed evidence in `TASK.md`.
2. Ask permission for the final unit commit.
3. If refused or absent, mark the task and unit `Blocked`, keep the active
   directory and ledger, and stop.
4. If granted, prepare the unit as `Done`, the task as `Completed`, and move the
   entire directory unchanged to `tasks/old/`.
5. Create the final unit commit with the required task and unit trailers.
6. Treat completion and archival as effective only after commit success; on
   failure, retain or restore the active `Blocked` state.
7. Delete the temporary execution ledger only after commit success.

Closure does not require a separate unit. For a one-unit task, its only commit
may introduce the complete record directly under `tasks/old/`.

## Artifact ownership after archival

The archived directory is the immutable durable task record. Keep internal
links relative so they survive the move. Correct later factual mistakes
explicitly rather than silently rewriting execution history.
