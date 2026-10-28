# System Architecture

This document is the entry point for the service's current architecture. Keep
it concise and update it when a change alters system boundaries, runtime
components, data ownership, or important integrations.

Do not use this document for proposed changes. Describe a proposed solution in
the task's `DESIGN.md`; after implementation, reconcile the accepted result
here when it changes the current architecture.

## System overview

Document only facts that apply to the service:

- its purpose, responsibilities, and explicit non-responsibilities;
- users and external systems at the boundary (C4 System Context);
- deployable applications, workers, datastores, and message brokers (C4
  Container);
- owned data and authoritative sources;
- a small number of critical request, event, or background-job flows;
- material availability, security, performance, and operational constraints.

Prefer one context diagram and one container diagram with brief supporting
text. Add a separate document only when this page becomes difficult to use or
a topic needs independent ownership and maintenance.

## Code architecture

Layer responsibilities, dependency direction, ports, adapters, and package
guidance are defined in [Clean Architecture](clean-architecture.md).

## Architecture decisions

Create `docs/architecture/decisions/` only when the first durable,
architecturally significant decision needs to be recorded. Keep each ADR short
and include its context, decision, status, and consequences. Do not create an
ADR for routine implementation choices or use ADRs as a substitute for this
current-state overview.

## References

- [C4 model: diagrams](https://c4model.com/diagrams)
- [arc42: architecture documentation](https://arc42.org/documentation/)
- [Michael Nygard: Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
