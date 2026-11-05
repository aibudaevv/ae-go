# AGENTS.md

Instructions for coding agents working on this Go service. Prefer small,
reviewable changes that preserve existing behavior, contracts, and operational
properties.

## Working rules

1. Inspect the relevant package, tests, configuration, contracts, and current
   working tree before editing. Preserve unrelated changes.
2. Read the contextual documentation in the table below when the task matches;
   do not load unrelated documents for a small change.
3. Do not introduce a framework, dependency, transport, datastore, or deployment
   mechanism unless the task requires it.
4. Keep production changes, database operations, credentials, deployment, commit,
   push, and other external writes behind explicit approval.
5. Never claim a check passed unless it was run and its result was observed.

## Task workflow

Before modifying the repository, classify the requested work as small or
non-trivial using the [Task Workflow](tasks/README.md).

Small work does not require task artifacts. For every non-trivial task:

1. Read `tasks/README.md` completely before implementation.
2. Run its prerequisite preflight against the current session's skill catalog.
   If it fails, stop before allocating an ID, creating a worktree, or creating
   task artifacts.
3. Use the task identifier supplied by the user. If none was supplied,
   allocate the next repository-wide, non-reusable `task-NNN` identifier from
   active tasks, `tasks/old/`, repository history, known refs, and worktrees.
4. Ask permission to create a worktree. If refused, continue in the current
   checkout. Then create `TASK.md` from `tasks/templates/TASK.md` and
   `GRILLING.md` from `tasks/templates/GRILLING.md`.
5. Review relevant open entries in `tasks/IMPROVEMENTS.md`, create `DESIGN.md`,
   invoke `$grilling` after its first draft, resolve every material question,
   and obtain design approval. Then create `PLAN.md`.
6. Set `Ready` only when all four task artifacts exist and the plan is
   actionable. Keep `TASK.md` as a compact status and evidence manifest; do not
   duplicate design, plan, or step-level progress in it.
7. Execute independently reviewable units of at most 300 changed lines. After
   verifying each unit, request permission for its single commit; do not start
   the next unit without a successful permitted commit.
8. Before completion, record the required compounding review in `TASK.md` using
   the outcomes and promotion rules in `tasks/README.md`.
9. Run final verification and review before the final unit commit. That commit
   may also set `Completed` and move the task intact to `tasks/old/`.

## Commands

Run commands from the module root unless repository-specific instructions say
otherwise.

```sh
gofmt -w <changed-go-files>
go build ./...
go test -race -count=1 ./...
go mod tidy -diff
golangci-lint run ./... # when .golangci.yml/.golangci.yaml exists
```

Use focused package tests while iterating, then run the broadest relevant checks
before handoff. If several `go.mod` files exist, verify every affected module;
root `./...` does not cover submodules. Report unavailable commands or required
external infrastructure instead of silently skipping a check.

## Required context by task

| When changing… | Read before editing… |
| --- | --- |
| planning, estimating, or decomposing a task | [Task Planning and Review Units](docs/development/planning.md) |
| any `.go` file, package API, names, declarations, control flow, or formatting | [Go Style](docs/development/go-style.md) |
| system boundaries, runtime components, data ownership, or important integrations | [System Architecture](docs/architecture/README.md) |
| layer boundaries, use cases, ports, adapters, dependency wiring | [Pragmatic Layered Architecture](docs/architecture/clean-architecture.md) |
| decision logic or tests | [Table-Driven Tests and Mc/DC](docs/testing/mcdc.md) |
| HTTP, gRPC, protobuf, events, queues, retries, or idempotency | [HTTP, RPC, and Event Contracts](docs/contracts/http-rpc-events.md) |
| SQL, storage, schema, backfill, or migrations | [Data Access and Migrations](docs/data/migrations.md) |
| authentication, authorization, secrets, personal data, dependencies, or untrusted input | [Security and Privacy](docs/security.md) |
| logs, metrics, traces, health checks, alerts, dashboards, or SLOs | [Observability and Health](docs/operations/observability.md) |
| creating, amending, squashing, or proposing a commit message | [Commit Messages](docs/development/commits.md) |

## Non-negotiable engineering rules

- Split implementation plans into logically complete, independently reviewable
  units of no more than 300 changed lines. Follow the documented exceptions and
  record the reason when a unit cannot be split safely.
- Follow the documented pragmatic layered architecture. Keep request handling
  in controllers, workflows in services, persistence selection in domain/data
  gateways, and SQL plus driver connections in database packages. Gateways may
  use shared DTOs, persistence operations, and the database executor. Do not add
  new controller or service dependencies on database models; existing cases are
  legacy exceptions. Keep transport and provider SDK types inside adapters.
- Write table-driven tests. For every changed non-trivial decision, demonstrate
  Mc/DC: each atomic condition independently changes the outcome, or a documented
  domain invariant explains why independent variation is impossible.
- Pass `context.Context` first for request-scoped or cancellable work. Never pass
  `nil` or store a context in a struct.
- Wrap inspectable errors with operation context and `%w`; do not discard errors,
  panic on external input, or call `log.Fatal` outside process bootstrap.
- Every goroutine needs an owner, cancellation path, error path, and shutdown
  behavior. The owner closes resources it opens unless ownership is transferred.
- Never put secrets or sensitive user data in source, fixtures, logs, errors,
  traces, documentation, or generated artifacts.

## Definition of done

A change is complete only when all applicable conditions hold:

1. It implements the requested behavior and stays within approved scope.
2. Relevant table-driven tests, Mc/DC pairs, and integration or contract checks
   cover changed behavior; a new regression test fails without the source change.
3. Relevant build, test, module, lint, migration, and integration checks ran.
   If a required check is unavailable, report the check and reason; completion
   requires the user's explicit acceptance of the remaining risk.
4. Contracts, migrations, configuration examples, metrics, dashboards,
   alerts, or documentation affected by the change are updated.
5. The diff has no unrelated refactoring, generated noise, secrets, or accidental
   dependency changes, and `git diff --check` passes.

## Review before handoff

Inspect the final diff and ask:

1. Did this alter an API, event, schema, authorization rule, or operational
   contract?
2. Could a retry, timeout, duplicate message, partial failure, or shutdown
   corrupt state or produce an incorrect result?
3. Are errors actionable without leaking data?
4. Did the required checks actually run and prove the changed behavior?
