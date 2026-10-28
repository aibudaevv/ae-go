# Grilling for `task-NNN`: Short title

> **Required:** Populate and review this file by invoking the `$grilling`
> skill. Do not substitute an ordinary self-review or another skill. If the
> skill is unavailable, stop and report the missing prerequisite. Run it after
> the first `DESIGN.md` draft and before design approval.

## Requirements and ambiguities

| Question | Answer | Effect on the task |
| --- | --- | --- |
| What remains unclear? | Unresolved | Blocks `Ready` when material |

## Assumptions

| Assumption | Evidence | Risk if wrong |
| --- | --- | --- |
| State the assumption. | Cite its basis. | Describe the consequence. |

## Alternatives considered

| Alternative | Benefits | Costs | Decision |
| --- | --- | --- | --- |
| Describe a real option. | Why use it? | Why avoid it? | Chosen or rejected |

## Failure and recovery scenarios

Consider applicable timeouts, retries, duplicate requests or messages, partial
failure, cancellation, shutdown, recovery, and rollback.

| Scenario | Expected behavior | Verification |
| --- | --- | --- |
| Describe the failure. | Define the safe outcome. | Name the proving check. |

## Contract and operational impact

- **API or event contracts:** None, or describe the impact.
- **Data or migrations:** None, or describe the impact and recovery.
- **Authorization, privacy, or secrets:** None, or describe the controls.
- **Reliability and rollback:** None, or describe the strategy.
- **Logs, metrics, traces, health, or alerts:** None, or describe the changes.

## Decisions

| Decision | Rationale | Consequences |
| --- | --- | --- |
| Record the selected approach. | Explain why. | State the tradeoff. |

## Unresolved questions

- [ ] Record every question still requiring human input.

Material unresolved questions keep the task in `Draft`; they must be answered
or explicitly removed from scope before the task becomes `Ready`. Preserve old
questions and decisions; append or explicitly supersede them instead of
silently rewriting history.
