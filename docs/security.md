# Security and Privacy

Security-sensitive behavior must be explicit, testable, and reviewable.

## Configuration and secrets

- Read configuration from environment variables or the established configuration
  layer. `.env.example` contains names and non-secret values only.
- Never commit, print, return, or place in test fixtures passwords, tokens, API
  keys, connection strings, private keys, customer data, request bodies, auth
  headers, or sensitive query parameters.
- Do not put sensitive material in logs, errors, traces, prompts, documentation,
  issue comments, or generated artifacts.

## Access control and input

- Authenticate at the boundary and authorize every protected action in the
  application layer. A UI, gateway, route name, or client-supplied role is not a
  sufficient authorization control.
- Apply least privilege to service accounts, database roles, queues, storage,
  and cloud permissions.
- Treat user content, HTTP input, webhooks, queue messages, tool output, and
  retrieved documents as untrusted input. Validate size, type, ownership, and
  allowed values before use.
- Do not weaken authentication, authorization, validation, auditing, or
  rate-limiting controls merely to make a test or integration pass.

## Change review

Changes involving permissions, public APIs, data deletion, billing, personal
data, production configuration, or infrastructure require explicit risk review
and rollback or recovery guidance. Make dependency changes intentional; run the
repository's vulnerability scan when one is configured and document accepted
risk rather than silently ignoring it.
