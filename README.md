# Go Service Engineering Harness

This repository is a file-based engineering harness template for Go services.
It defines a reviewable way to design, plan, implement, and verify changes, but
is not a runnable Go service and contains no Go module or service source code.

## What is included

- [`AGENTS.md`](AGENTS.md) is the agent-facing entry point for working rules,
  approval boundaries, engineering constraints, and contextual documentation.
- [`tasks/README.md`](tasks/README.md) defines the task workflow, artifact
  lifecycle, checkpoints, review units, and completion rules.
- [`docs/`](docs/) contains focused guidance for architecture, contracts, data,
  development, operations, security, and testing.
- [`tasks/templates/`](tasks/templates/) provides starting points for the task
  manifest and grilling record.

Detailed commands, lifecycle rules, and engineering policies remain in those
documents rather than being repeated here.

## Prerequisites

Codex is the reference runtime for non-trivial work. The workflow requires the
`superpowers:brainstorming`, `superpowers:writing-plans`, and `grilling` skills
to be discoverable in the current session. Follow the installation and
capability preflight in the
[`Verify prerequisites`](tasks/README.md#verify-prerequisites) section before
allocating a task identifier or creating task artifacts.

## Adopt the harness in an existing service

Treat this repository as source material, not as a replacement documentation
tree. Selectively merge the relevant rules and templates with the service's
existing documentation; do not overwrite established guidance blindly.

During adoption, reconcile `AGENTS.md` with existing contributor and agent
instructions.

1. Add the task workflow and templates where the team wants durable task
   records.
2. Select only the topical documents that match the service's technologies and
   operational responsibilities.
3. Adapt build, test, lint, migration, and integration commands to the actual
   module layout and toolchain.
4. Replace generic architecture guidance and placeholders with verified package
   boundaries, dependencies, owners, contracts, and operational facts.

Keep service-specific documentation authoritative when it is more precise, and
resolve conflicts explicitly before relying on the combined instructions. This
README is an orientation page for the template and does not need to be copied
into the target service.

## Run a first trial

Choose a low-risk but non-trivial change that can exercise the complete workflow
without production impact. Follow [`tasks/README.md`](tasks/README.md) and create
the four task artifacts: `TASK.md`, `DESIGN.md`, `GRILLING.md`, and `PLAN.md`.
Use the trial to confirm that approvals, checkpoints, review-unit sizing, skills,
and verification commands work for the team before adopting the harness more
broadly.
