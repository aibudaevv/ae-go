# HTTP, RPC, and Event Contracts

Treat every external boundary as a compatibility contract. This includes HTTP
requests and responses, gRPC/protobuf messages, queue topics and payloads,
webhooks, scheduled jobs, and commands consumed by another service.

## Boundary behavior

- Validate untrusted input at the inbound adapter and convert it into typed
  application input before invoking a use case.
- Return stable, documented response and error shapes. Do not expose SQL errors,
  provider responses, stack traces, secrets, or internal topology.
- Keep transport-specific concerns in adapters. Domain and application code do
  not receive `http.Request`, generated protobuf messages, or broker payloads.
- Set explicit timeouts for outbound HTTP, RPC, database, and broker work.
  Propagate `context.Context` cancellation and classify timeout, retryable, and
  permanent failure paths.

## Compatibility

Prefer additive evolution: add optional fields, endpoints, message versions, or
event attributes before removing old behavior. Treat changes to request/response
fields, error codes, enum meaning, event schemas, topics, consumers, and retry
semantics as compatibility changes.

Before a breaking change, identify every known producer and consumer, define a
migration window, update the canonical API/schema specification, and provide a
rollback or forward-recovery path.

## Delivery and retries

Assume network requests may time out after the remote side performed the work,
and messages may arrive more than once or out of order. Make write handlers,
consumers, and jobs idempotent where retries or at-least-once delivery are
possible. Use durable idempotency keys or deduplication when correctness needs
them.

Bound request body size, pagination, fan-out, queue depth, retry count, and
backoff. Do not retry unsafe writes blindly; retry only failures and operations
whose semantics make another attempt safe. Capture enough correlation metadata
to diagnose a failed contract without recording sensitive payloads.
