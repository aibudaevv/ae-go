# Grilling for `task-001`: Add a compounding learning loop

> **Required:** This record is populated through the `$grilling` skill after
> the first `DESIGN.md` draft and before design approval. Material unresolved
> questions keep the task in `Draft`.

## Requirements and ambiguities

| Question | Answer | Effect on the task |
| --- | --- | --- |
| What happens to a relevant `Ready` item outside current scope? | It does not block completion; record the reason and next trigger, then reconsider it in every later relevant design. | Prevents silent scope growth without losing accountability. |
| Do positive patterns use the same promotion threshold? | Yes; they need two independent task sources. | The single-source fast path remains limited to serious risks. |

## Assumptions

| Assumption | Evidence | Risk if wrong |
| --- | --- | --- |
| The primary runtime is Codex, while documents remain human-readable. | User selected Codex-first. | Routing could overfit one agent runtime. |
| Version 1 is process-only. | User excluded local automation and CI. | The loop initially relies on disciplined execution. |
| Review is required for every non-trivial task, but improvement is conditional. | User selected conditional review. | Weak evidence could otherwise create process noise. |

## Alternatives considered

| Alternative | Benefits | Costs | Decision |
| --- | --- | --- | --- |
| Task-local review plus deferred register | Low overhead and traceable context | One shared register must be maintained | Chosen |
| Per-task `LEARNINGS.md` | Rich task history | Empty files and duplicated authority | Rejected |
| Central register for every signal | Easy aggregate view | Loses task-local closure context | Rejected |

## Failure and recovery scenarios

| Scenario | Expected behavior | Verification |
| --- | --- | --- |
| Task has no reusable learning | Record reasoned `No action`; completion remains possible | Closure scenario review |
| Same ordinary signal appears again | Update existing entry with a second task source and set `Ready` | Mc/DC scenario review |
| Serious risk appears once | Register it as `Ready` immediately | Mc/DC scenario review |
| Promotion is outside approved scope | Do not change unrelated policy in the current task | Diff and scope review |
| Two tasks create similar candidates | Merge evidence into one entry when discovered; preserve both sources | Registry review |

## Contract and operational impact

- **API or event contracts:** None.
- **Data or migrations:** None.
- **Authorization, privacy, or secrets:** The policy recognizes serious risks;
  no sensitive values belong in the register.
- **Reliability and rollback:** Documentation-only change; revert the relevant
  unit commit if the workflow proves unusable.
- **Logs, metrics, traces, health, or alerts:** None.

## Decisions

| Decision | Rationale | Consequences |
| --- | --- | --- |
| Use a thin closed loop. | It fits the existing four-artifact model. | Deferred items need one central register. |
| Use risk or two independent sources as the threshold. | It reacts quickly to severe failures without promoting every idea. | Source independence must be explicit. |
| Keep v1 Markdown-only. | The user selected process before automation. | Compliance is verified by review rather than CI. |
| Allow reasoned deferral of out-of-scope `Ready` entries. | A learning backlog must not silently expand approved task scope. | Every later relevant design must reconsider the entry. |
| Apply the ordinary threshold to successful patterns. | One successful use may be contextual rather than reusable. | Positive patterns need two distinct task IDs. |

## Unresolved questions

- [x] Decide how a relevant out-of-scope `Ready` item affects task completion.
- [x] Decide whether successful patterns and failure signals share a threshold.

No material questions remain unresolved.
