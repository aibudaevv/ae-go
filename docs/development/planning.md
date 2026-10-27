# Task Planning and Review Units

Read this document before estimating, decomposing, or writing an implementation
plan. Break work into units that are logically complete, independently
verifiable, and convenient to review.

## Unit definition

A planning unit is the smallest coherent deliverable that a reviewer can
understand, verify, accept, or reject independently. Depending on the change, a
unit may contain production code, its tests, contract updates, migrations, and
the documentation required to make that behavior complete.

A unit is not:

- an arbitrary slice of a file or diff;
- a development phase such as "write all code" or "add all tests";
- a collection of unrelated small changes combined to approach the size limit;
- necessarily the same thing as a Go unit test or a Git commit.

## Size limit

Each planned unit must contain no more than 300 changed lines:

```text
changed lines = additions + deletions
```

The limit is a reviewability guardrail, not a target. A complete unit may be 10,
50, or 200 lines. Do not add work or combine concerns to make a unit larger.

Estimate the changed lines during planning and verify the actual size after
implementation. When implementation crosses the limit, stop at the next safe
boundary and split the remaining work into another unit.

## Cohesion and independence

Every unit must:

1. have one clear purpose and observable completion condition;
2. preserve a buildable and internally consistent repository state;
3. include the tests needed to verify its behavior;
4. include directly required contract, schema, configuration, or documentation
   changes;
5. avoid depending on unfinished details hidden in a later unit;
6. be small enough for a reviewer to understand without reconstructing the
   entire feature.

A later unit may build on an earlier completed unit. State that dependency in
the plan and define the interface or behavior that connects them.

## How to split work

Prefer boundaries that produce meaningful vertical or behavioral slices:

- one use case or independently observable behavior;
- one contract extension and its implementation;
- one schema expansion before a later data migration or cleanup;
- one application port together with the adapter that proves it;
- one refactoring that preserves behavior before a subsequent behavior change;
- one failure mode or compatibility path with its tests.

Avoid splitting solely by technical layer when it leaves intermediate units
unverifiable. For example, "add all repository methods" followed by "add all
services" is weaker than a sequence of complete use cases that each crosses the
necessary layers.

Do not separate tests from the behavior they verify merely to keep the
production-code diff below the limit. Reduce the behavior included in the unit
instead.

## Lines counted separately

Generated files, dependency lock files, formatted fixtures, snapshots, and
mechanical renames can dominate a diff without increasing its conceptual size.
List them separately from the hand-written change:

```text
Unit: add event visibility validation
Hand-written change: approximately 180 lines
Generated/mechanical change: approximately 640 lines
```

Do not use this distinction to hide hand-written complexity. Generated or
mechanical output must be reproducible, reviewable for unexpected changes, and
directly caused by the unit.

## Exceptions

Exceeding 300 hand-written changed lines is allowed only when splitting would:

- leave the repository unbuildable or the data model inconsistent;
- separate a contract from the minimum implementation needed to validate it;
- make a migration unsafe;
- duplicate substantial temporary code;
- reduce reviewability more than the larger cohesive unit would.

Before implementation, record the exception in the plan with:

- the estimated size;
- why the unit is indivisible;
- the alternative splits considered;
- how the reviewer should verify it;
- the primary review risks.

"The change is large," "the files are related," or "splitting takes longer" is
not sufficient justification.

## Plan format

Describe each unit using a compact contract:

```text
Unit: <coherent outcome>
Purpose: <one behavior or structural result>
Estimated size: <additions + deletions, excluding listed generated changes>
Inputs/dependencies: <earlier units or existing contracts>
Produces: <behavior, interface, schema, or artifact>
Verification: <tests and checks proving completion>
Generated/mechanical changes: <files and estimated size, or none>
```

If the estimate is uncertain, use a range whose upper bound does not exceed
300. Split the unit further when a reasonable estimate cannot stay within that
bound.

## Review checklist

Before approving a plan, verify that:

1. every unit is logically complete and independently verifiable;
2. each unit has one clear purpose and explicit completion evidence;
3. every hand-written unit is estimated at no more than 300 changed lines;
4. tests remain with the behavior they verify;
5. dependencies and ordering between units are explicit;
6. generated and mechanical changes are disclosed separately;
7. every exception contains a concrete indivisibility argument and review plan.
