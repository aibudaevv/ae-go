# `task-001`: Compounding Learning Loop Design

## Intent

Turn the existing task harness into a Codex-first compounding engineering
system. Every completed non-trivial task should evaluate whether its observed
work produced reusable knowledge, without forcing artificial process changes.

## Goals

- Require a short evidence-based compounding review before task completion.
- Promote reusable knowledge into one canonical owner.
- Preserve unpromoted signals in one discoverable improvement register.
- Make later task designs consider relevant accumulated knowledge.
- Keep small standalone work lightweight.

## Non-goals

- Add scripts, CI, executable evals, repo-local skills, or dependencies.
- Create Go service code or change runtime behavior.
- Require every task to invent a permanent improvement.
- Scan the full archived-task history at the start of every task.
- Silently expand an approved task to implement an unrelated improvement.

## Approach

Use a thin closed loop within the existing four-artifact task model:

1. During design, inspect relevant open entries in `tasks/IMPROVEMENTS.md` and
   record their disposition during grilling.
2. Before completion, add a compact compounding review to `TASK.md`.
3. Give each material signal one result: `No action`, `Promoted`, or
   `Deferred`.
4. Promote in-scope knowledge into one canonical owner; register out-of-scope
   or insufficiently proven knowledge as a deferred candidate.

This keeps `TASK.md` a compact evidence manifest. `tasks/README.md` owns the
detailed lifecycle, while `tasks/IMPROVEMENTS.md` owns the deferred backlog.

## Improvement register

Each deferred entry has a stable ID `<source-task-id>-I<N>` and records:

- status;
- area or trigger;
- independent evidence sources;
- proposed canonical target;
- decision or next action.

Statuses are `Candidate`, `Ready`, `Promoted`, and `Rejected`. A later task
adds its task ID to an existing matching entry instead of creating a duplicate.

The readiness decision is:

```text
serious risk OR at least two independent task sources
```

A serious risk is a security or privacy exposure, data loss or corruption,
public-contract break, escaped defect, or inability to run a mandatory check.
Two observations inside one task remain one independent source.

## Promotion ownership

- Always-loaded policy and routing belong in `AGENTS.md`.
- Topic-specific engineering knowledge belongs in the matching `docs/` owner.
- Artifact shape belongs in `tasks/templates/`.
- Task lifecycle mechanics belong in `tasks/README.md`.
- Potential scripts, skills, CI, or evals remain deferred candidates in v1.

Cross-links may aid discovery, but the rule itself has one canonical owner.
Promotion may occur in the current task only when it is inside the approved
design; otherwise the task records a deferred follow-up instead of expanding
scope.

## Task lifecycle changes

The first design draft considers open register entries whose area or trigger
matches the proposed scope. The subsequent grilling record captures whether
each relevant entry changes the design, is unrelated, or remains deferred.
Encountering a relevant `Ready` entry outside the current scope does not block
completion. Its grilling disposition must state why it remains out of scope and
the next trigger that requires reconsideration. Each later relevant design must
again choose promotion, rejection, or a reasoned deferral.

Before `Completed`, `TASK.md` records the material signals reviewed and their
outcomes. `No action` requires a reason, `Promoted` links the canonical target,
and `Deferred` links a register ID. An absent review blocks completion. Small
standalone tasks continue to create no mandatory task artifacts.

Successful reusable patterns use the same two-independent-source threshold as
ordinary failure-derived signals. The single-source fast path is reserved for
serious risks.

## Alternatives considered

1. Add `LEARNINGS.md` to every task. Rejected because most files would be nearly
   empty and the fifth artifact would duplicate the compact manifest.
2. Store every signal only in a central register. Rejected because it loses the
   source task's closure context and turns the register into a work log.
3. Keep the task-local review plus a central register only for deferred items.
   **Chosen** because it closes the loop with minimal process overhead.

## Acceptance criteria

- **AC-1:** Every non-trivial task records a compounding review before it can
  become `Completed`; an evidence-based `No action` is valid.
- **AC-2:** Deferred improvements have stable IDs, evidence sources, triggers,
  proposed canonical targets, decisions, and one documented lifecycle.
- **AC-3:** A serious single-task risk becomes `Ready`; an ordinary signal
  becomes `Ready` only after two independent task sources.
- **AC-4:** Later task design and grilling consider relevant open improvements
  without scanning every archived task.
- **AC-5:** Promotion updates exactly one canonical owner and does not silently
  expand approved scope.
- **AC-6:** `AGENTS.md` routes agents into the loop without duplicating the
  detailed lifecycle.
- **AC-7:** Small standalone work remains exempt, and v1 adds no executable
  automation, dependencies, or Go service code.

## Resolved decisions

- A relevant out-of-scope `Ready` entry does not block completion, but every
  relevant design must record a disposition and next reconsideration trigger.
- Successful patterns require two independent task sources; only serious risks
  use the single-source fast path.
