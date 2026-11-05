# Deferred Improvements

This register owns reusable learning signals that have not yet been promoted
into canonical repository guidance. The lifecycle and readiness rules are
defined in the [task workflow](README.md#compound-engineering-loop).

## Entry contract

- IDs use `<source-task-id>-I<N>`, where `N` is the next sequence number within
  the task that first records the signal.
- Evidence sources contain distinct task IDs. Multiple observations within one
  task count as one source.
- Status is `Candidate`, `Ready`, `Promoted`, or `Rejected`.
- Area or trigger is specific enough for a later design to decide relevance.
- Proposed target names the single canonical document, template, or workflow
  that would own the promoted rule.
- Decision records the next reconsideration trigger, promoted target, or
  rejection rationale.
- Entries contain no secrets, sensitive personal data, or unnecessary incident
  details.

## Open and resolved improvements

| ID | Status | Area or trigger | Evidence sources | Proposed canonical target | Decision or next trigger |
| --- | --- | --- | --- | --- | --- |
| `task-001-I1` | `Candidate` | Superpowers inline execution helper paths and `Task N` versus `Unit N` plan headings | `task-001` | `tasks/README.md` execution guidance | Reconsider if a second task reproduces either compatibility failure; keep external plugin details out of always-loaded guidance until then. |
