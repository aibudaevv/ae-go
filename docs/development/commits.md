# Commit Messages

Read this document before creating, amending, squashing, or proposing a commit
message. Do not create a commit unless the user or repository workflow has
explicitly authorized it.

Commit messages follow
[Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/)
with one mandatory extension: the subject contains the task identifier.

## Required subject format

```text
<type>[optional scope][!]: <TASK-ID> <description>
```

Examples:

```text
feat(api): TASK-123 add event creation endpoint
fix(storage): TASK-456 prevent duplicate records
docs: TASK-612 clarify migration procedure
refactor(core)!: TASK-789 replace legacy contract
```

Rules:

- `type` is mandatory and lower-case.
- `scope` is optional. When present, it is a short noun naming the affected
  area, not the task identifier.
- `!` immediately before `:` marks a breaking change.
- `TASK-ID` is mandatory and uses the identifier provided by the task tracker
  or user. Do not invent an identifier.
- `description` is mandatory, concise, imperative, lower-case, and has no
  trailing period.

The type must remain at the beginning so Conventional Commits tooling can parse
the message. These subjects are invalid:

```text
TASK-123 feat(api): add event creation endpoint
feat(TASK-123): add event creation endpoint
feat(api): add event creation endpoint
```

The first puts non-standard text before the type, the second consumes the scope
with tracking metadata, and the third omits the mandatory task identifier.

If no task identifier is available, ask for it before committing. Do not use
placeholders such as `NO-TICKET`, `TASK-000`, or `XXX` unless repository-specific
instructions explicitly define such a convention.

## Types

Use `feat` for a new user-visible capability and `fix` for a bug fix. These are
the types with defined Semantic Versioning meaning in Conventional Commits.

Common additional types are:

- `docs` — documentation only;
- `refactor` — code restructuring without a feature or bug fix;
- `test` — tests only;
- `perf` — performance improvement;
- `build` — build system or dependency changes;
- `ci` — CI configuration or automation;
- `chore` — necessary maintenance that fits no more specific type;
- `revert` — reversal of an earlier commit.

Prefer the most precise type. Do not use `chore` as a default for ordinary
product work.

## Scope

Use a scope when it helps a reader locate the change:

```text
fix(auth): TASK-456 reject expired sessions
feat(events): TASK-123 support private events
```

Keep scope names stable and consistent with the codebase. Omit the scope when a
change spans several areas or no concise scope improves the message.

## Body and footers

The subject states what changed. Add a body when the motivation, behavior,
tradeoff, migration, or operational consequence is not obvious. Separate the
body from the subject with one blank line:

```text
fix(storage): TASK-456 prevent duplicate records

Serialize competing writes through the existing transaction so retries cannot
create a second record.
```

Place footers after another blank line. Use standard Git-trailer form where
possible:

```text
feat(api): TASK-123 add event creation endpoint

Expose creation through the public HTTP contract and preserve idempotent
retries.

Refs: TASK-123
Reviewed-by: Example Reviewer
```

The task identifier must remain in the subject even when it is repeated in a
footer.

## Breaking changes

Mark a breaking change with `!`, a `BREAKING CHANGE:` footer, or both. The
footer explains the incompatibility and required migration:

```text
feat(api)!: TASK-789 replace legacy event response

BREAKING CHANGE: clients must read the event identifier from `id` instead of
`event_id`.
```

Do not label an ordinary internal refactor as breaking. A change is breaking
when a supported consumer must change to remain compatible.

## Commit boundaries

- One commit represents one coherent change and leaves the repository in a
  valid state.
- Do not mix unrelated formatting, refactoring, generated output, or dependency
  updates into a feature or fix.
- Include tests and documentation required by the behavior in the same commit
  unless the repository's review workflow requires another split.
- Split changes that need different types or task identifiers whenever
  practical.
- For a squash workflow, the resulting squash commit—not every temporary local
  commit—must satisfy this policy unless repository-specific rules say otherwise.

## Pre-commit check

Before creating or proposing a commit, confirm that:

1. explicit authorization to commit exists;
2. the subject matches the required grammar;
3. the task identifier is real and appears in the subject;
4. the type and optional scope describe the actual diff;
5. breaking behavior is marked and explained;
6. the commit contains one coherent, verified change.
