# Commit Messages

Read before creating, amending, squashing, or proposing a commit message.
Creating a commit requires explicit authorization. Design approval alone does
not authorize it; honor task-wide permission already granted in the session.

## Subject

Follow [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/):

```text
<type>[optional scope][!]: <TASK-ID> <description>
<type>[optional scope][!]: <description>
```

- Keep the lower-case type first. A task ID never replaces the type or scope.
- Include the real task ID for recorded work and whenever the user supplies one.
  Small standalone work without a supplied ID uses the second form; generate
  no ID or placeholder for it.
- Scope is an optional, short, stable area name. Omit it when none helps.
- Description is concise, imperative, lower-case, and has no trailing period.

```text
fix(storage): TASK-456 prevent duplicate records
docs: clarify migration procedure
```

Use `feat` for a capability and `fix` for a bug; these carry the standard SemVer
meaning. Other types are `docs`, `refactor`, `test`, `perf`, `build`, `ci`,
`chore`, `revert`. Prefer the precise type; do not default to `chore`.

## Body and trailers

Add a body when motivation, behavior, tradeoffs, migration, or operational
impact is not obvious. Separate subject, body, and trailers with blank lines.
Use standard Git trailers where possible. A task ID in a footer does not
replace the required subject ID. Recorded unit commits use the `Task` and
`Unit` trailers specified in the [workflow](../../tasks/README.md#execute-a-unit).

Mark supported-consumer incompatibility with `!`, a `BREAKING CHANGE:` footer,
or both. The footer explains the incompatibility and required migration.
An ordinary internal refactor is not a breaking change.

## Boundaries and verification

- One commit contains one coherent change and leaves the repository valid.
- Include directly required tests and documentation with the behavior unless
  the repository review workflow requires another split.
- Keep unrelated formatting, refactoring, generated output, and dependency
  updates separate; split different types or task IDs when practical.
- In a squash workflow, the final squash commit must satisfy this policy;
  temporary commits need not unless repository instructions require it.
- Before committing, confirm permission, subject grammar, real ID when needed,
  accurate type/scope, breaking-change marking, and a reviewed, verified diff.

Commit success is a Git result, separate from work verification. Observe it
before reporting success; see
[local authorization](../../tasks/README.md#local-commit-authorization).
