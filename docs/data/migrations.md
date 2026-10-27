# Data Access and Migrations

Persistence is an outer concern. Keep SQL, ORM/database models, driver errors,
and connection management in adapters or platform code; convert them at the
application boundary instead of leaking them into domain contracts.

## Data access

- Parameterize every query. Never build SQL by interpolating external input.
- Set a context deadline for database work and close rows, statements, and
  transactions in the layer that opened them.
- Map constraint and driver errors to stable application errors; do not expose
  database internals through an API response.
- Keep transaction scope as small as possible while preserving the complete
  use-case invariant.

## Migration safety

Every schema migration must be ordered, reviewed, compatible with existing
production data, and have a rollback or forward-recovery plan. Do not assume a
migration runner makes an operation reversible or safe under live traffic.

For deploy-spanning changes, use expand/contract:

1. Add a backward-compatible schema, index, or column.
2. Deploy code that safely reads and writes both representations.
3. Backfill with bounded, observable, resumable work.
4. Switch reads only after verifying the backfill and rollback posture.
5. Remove obsolete fields in a later, separately reviewed change.

Test migrations against a representative prior schema and data volume when the
change can lock a table, rewrite data, alter a high-traffic query, or affect a
critical invariant. A destructive migration, data repair, or production query
requires explicit approval and a verified target environment.
