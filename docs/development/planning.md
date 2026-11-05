# Task Planning and Review Units

Read before estimating, decomposing, or writing an implementation plan.
For ordinary changes keep the plan short; full-design work starts from its
approved design. Task routing and approvals belong to the
[workflow](../../tasks/README.md).

## Review units

A unit is one coherent outcome that can be understood, verified, accepted,
or rejected independently. Include its code, tests, and directly required
contract, schema, configuration, and documentation changes together.
It is neither an arbitrary diff slice nor necessarily a Git commit.

Every unit must:

- have one purpose and an observable completion condition;
- preserve a buildable, internally consistent repository;
- include the tests proving its behavior and required supporting changes;
- avoid unfinished details hidden in a later unit.

Prefer complete behavioral slices: one use case, contract extension with its
implementation, failure mode with tests, or port with the adapter proving it.
A behavior-preserving refactor may precede a behavior change; schema expansion
may precede backfill and cleanup. State dependencies and the connecting
interface or behavior. Avoid layer-only splits that cannot be verified alone.
Do not separate tests from their behavior to meet the size limit.

## Size and accounting

Each planned unit has at most 300 hand-written changed lines
(additions + deletions). This is a guardrail, not a target: do not add work or
combine unrelated concerns to fill it.

Estimate during planning and verify actual size after implementation. Use a
range only when its upper bound stays within 300. If an estimate cannot fit,
split further; if implementation crosses the limit, stop at the next safe
boundary and split the remaining work.

List generated files, lock files, formatted fixtures, snapshots, and mechanical
renames separately with their sizes. They must be reproducible, directly
caused by the unit, and reviewed for unexpected changes. This distinction must
not hide hand-written complexity.

## Exceptions

A larger cohesive unit is allowed only when splitting would leave the build or
data model inconsistent, separate a contract from its minimum validating
implementation, make a migration unsafe, duplicate substantial temporary code,
or reduce reviewability more than the larger unit would.

Before implementation record estimated size, why it is indivisible,
alternative splits considered, verification approach, and primary review risks.
Size, related files, or splitting effort alone do not justify an exception.

## Plan format and review

Use a compact table or equivalent fields for each unit:

```text
Unit and purpose: <one coherent outcome and completion condition>
Files and result: <changed files; behavior, interface, schema, or artifact>
Dependencies: <earlier units and connecting contracts, or none>
Size: <estimated additions + deletions>
Steps and verification: <concrete actions, commands, expected results>
Generated/mechanical output: <files and size, or none>
```

Review once for coherent outcomes, tests kept with behavior, explicit ordering,
complete acceptance coverage, size accounting, and justified exceptions.
