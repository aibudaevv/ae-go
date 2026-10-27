# Observability and Health

Production behavior must be diagnosable without exposing customer content or
secrets. Instrument the service at externally visible boundaries and critical
internal work.

## Logs

Use structured logs with stable keys. Include request, trace, tenant, or job IDs
only when the identifier is safe to record. Log operation name, outcome,
duration, and safe error classification; do not log sensitive payloads, tokens,
headers, or raw queries.

## Metrics and traces

Instrument relevant operations with metrics for request volume, duration,
errors, retries, saturation, queue depth, and dependency failures. Use stable
metric names and bounded label cardinality. Propagate trace context across HTTP,
RPC, jobs, and message consumers through the repository's telemetry library.

## Health and operations

Expose liveness and readiness separately:

- liveness answers whether the process is alive and should be restarted;
- readiness answers whether it can safely receive traffic.

Do not make a liveness probe depend on a transient remote dependency. A
production-impacting change must preserve or update the relevant dashboards,
alerts, SLOs, and rollback or incident guidance.
