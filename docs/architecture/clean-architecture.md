# Pragmatic Clean Architecture for Go Services

This document defines a reusable layered architecture for Go services. It is
compatible with Clean Architecture while remaining practical for ordinary HTTP
services, background workers, persistence, and external integrations.

It does not require a textbook-pure domain model or a large refactoring before
every product change. Prefer clear responsibilities, explicit dependencies,
and incremental improvement over package names that merely resemble an
architecture diagram.

## Request path

A typical synchronous request follows this path:

```text
transport and middleware
    -> controller
    -> application service
    -> domain/data gateway
    -> persistence operation
    -> database adapter
    -> datastore
```

Application services may also call external-system adapters, such as object
storage, caches, message brokers, payment providers, email, SMS, or push
notifications.

In this pragmatic variant:

- controllers are inbound transport adapters;
- services implement application workflows;
- domain/data gateways expose feature-oriented persistence operations;
- persistence models own database-specific commands and queries;
- infrastructure integrations remain at the edge of the application.

The `domain` package does not have to be a pure enterprise-domain layer. In an
existing service it may be a persistence-facing gateway. If so, describe and
preserve that role honestly instead of imposing a contradictory dependency
rule. A purer domain model can be introduced incrementally when it provides a
clear benefit.

## Example package layout

The names below are illustrative. Preserve equivalent established names when
working in an existing codebase.

| Location | Responsibility | Rules |
| --- | --- | --- |
| `cmd/<service>/` | Process entry point: load configuration, initialize logging, construct and run the application. | Keep only bootstrap and fatal startup handling here. |
| `internal/server/` | Composition root, routes and middleware, lifecycle management, migrations, scheduled jobs, and concrete dependency wiring. | Register transports and assemble dependencies here; do not add endpoint business policy. |
| `internal/controller/<feature>/` | Transport adapter: parse input, read authentication context, validate boundary data, invoke services, and map results or errors to responses. | Do not execute SQL or embed multi-step business policy. |
| `internal/service/<feature>/` | Application workflows: coordinate gateways, other services, integrations, and side effects. | Put authorization policy, sequencing, idempotency, retries, and compensating actions here. Depend on narrow interfaces. |
| `internal/domain/<feature>/` | Feature-oriented domain behavior or, in the pragmatic variant, a persistence gateway. | Do not put transport parsing, response writing, or provider SDK calls here. Keep its precise role consistent and documented. |
| `internal/dbmodel/` | Database commands, queries, row scanning, persistence-specific models, and stable persistence errors. | Keep SQL parameterized and database details inside this boundary. |
| `internal/dto/` | Shared request, response, and cross-layer data contracts where sharing is intentional. | Preserve external compatibility deliberately; do not leak transport-only or database-only details through DTOs. |
| `internal/platform/` or `pkg/` | Database mechanics and adapters for external systems. | Keep provider-specific types and calls at the infrastructure edge. |
| `internal/config/`, `internal/observability/` | Configuration, logging, metrics, tracing, and other cross-cutting plumbing. | Keep infrastructure plumbing out of feature workflows where possible. |

## Dependency direction

The expected dependency flow is:

```text
composition root -> controller -> service -> domain/data gateway
                                      |              |
                                      |              v
                                      |       persistence operation -> database adapter
                                      v
                              external-system ports -> provider adapters
```

The composition root may import and construct concrete implementations from all
layers. This is wiring, not a violation of the dependency rule.

Preserve these boundaries:

- A controller calls application-service interfaces. It does not construct SQL
  models or access the database adapter directly.
- A service owns the application flow. It calls feature gateways and external
  ports instead of embedding SQL or transport handling.
- A domain/data gateway selects persistence operations and returns domain- or
  contract-shaped results. It does not know HTTP request/response types or
  provider clients.
- A persistence operation owns SQL and row mapping for one query or command. It
  does not call services, controllers, or application workflows.
- A provider adapter owns provider-specific SDK details. Its consumers depend
  on a narrow interface representing the capability they need.

Dependencies must point toward policy. Infrastructure implements interfaces
owned by, or colocated with, the consuming application code whenever that
makes the dependency direction clearer.

## Adding or changing a feature

1. Locate the feature's transport, service, domain/data gateway, contracts,
   persistence operations, migrations, and end-to-end tests.
2. Change the external contract deliberately: update schemas, boundary
   validation, generated API documentation where applicable, and contract
   tests.
3. Add or change persistence operations at the database boundary; add an
   ordered migration for schema changes.
4. Extend the domain/data gateway that invokes those operations.
5. Add application-service behavior when the change involves policy,
   orchestration, another feature, an external system, retries, or cleanup.
6. Keep the controller focused on protocol translation, authentication-context
   extraction, input validation, and response or error mapping.
7. Wire concrete dependencies and register the transport in the composition
   root.

Do not introduce abstractions that add no meaningful boundary. A small,
read-only use case may need fewer layers when its behavior remains explicit and
testable. Conversely, do not place a multi-step workflow in a controller merely
because transport input is available there.

## Interfaces and contracts

Define interfaces around consumer needs, not around every concrete type. Keep
them small and cohesive. Avoid broad cross-feature manager interfaces and
interfaces introduced only to satisfy a mocking framework.

Shared DTOs may cross controller, service, and gateway boundaries when that is
the established convention. Treat them as contracts:

- prefer additive, backward-compatible changes;
- preserve the meaning of serialized fields;
- introduce conversion when transport or persistence details would otherwise
  leak across a boundary;
- keep database-only structures with persistence code;
- keep provider-specific structures inside provider adapters.

## Incremental evolution

New code may introduce a pure domain type or a use-case-specific port when it
materially reduces coupling. Pure inner code must not depend on SQL drivers,
HTTP frameworks, serialization libraries, or provider SDKs. An adapter at the
appropriate boundary implements its port.

Make architectural improvements around one coherent feature. Do not rename or
restructure unrelated packages solely to match a diagram, and do not combine a
broad architecture migration with an unrelated product change. Document any
intentional boundary exception and cover it with tests.

## Testing by layer

- Controller tests verify parsing, validation, authentication context,
  status/error mapping, and serialized contracts.
- Service tests verify policy, orchestration, permissions, side-effect order,
  idempotency, and failure or compensation behavior through ports or approved
  fakes.
- Domain/data-gateway and persistence tests verify database behavior, result
  mapping, transactions, constraints, and stable persistence errors.
- End-to-end tests verify externally observable workflows and contracts.

Use table-driven tests and apply Modified Condition/Decision Coverage (MC/DC)
at the layer that owns each decision. Do not duplicate identical branch cases
at every outer layer.
