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

Classify work using the [Task Workflow](tasks/README.md): read-only, ordinary
change, or full design. Read-only and small standalone work need no task
artifacts; other changes use one task record. Full design requires risk review
and design approval. Skills are optional tools, not prerequisites.
Keep task mechanics there, and honor workspace choices and approvals already
given in the current session.

## Verification

Select checks by changed behavior and run them from the relevant module root.

| When | Checks |
| --- | --- |
| During a unit | Format changed Go files; run focused package, regression, and applicable contract/integration checks |
| Final handoff after Go or module changes | Full command set below in every affected module; relevant migration and integration checks |
| Final handoff after documentation-only changes | Local Markdown links/anchors, affected examples and policy scenarios, and `git diff --check`; Go checks are inapplicable |

```sh
gofmt -w <changed-go-files>
go build ./...
go test -race -count=1 ./...
go mod tidy -diff
golangci-lint run ./... # when .golangci.yml/.golangci.yaml exists
```

Always review the final diff and run `git diff --check`. Root `./...` does not
cover submodules. Run the final applicable set once after implementation;
repeat affected checks after changes, failures, or new concerns. A commit or
bookkeeping alone does not require a rerun; hooks or generation changing tested
files do. Record commands, observed results, and the scope they verified.
Explain inapplicable checks. Report unavailable required checks or infrastructure;
they require explicit acceptance of remaining risk under the definition of done.

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
