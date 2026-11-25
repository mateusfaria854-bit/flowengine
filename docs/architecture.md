# Architecture

Status: proposed design. Implementation details may evolve as the MVP is validated.

## Principles

- A deterministic, framework-independent execution core.
- Keep HTTP request handling separate from background work.
- PostgreSQL is the source of truth; Redis is a transport, not the permanent execution ledger.
- Every run is associated with an immutable workflow revision and a tenant.
- Prefer a modular monorepo over distributed microservices until scale justifies them.

## Hosted SaaS boundaries

The same core should support self-hosted Community deployments and a managed Cloud offering. Keep hosting-specific operations separate from the engine. This is an architectural boundary, not a claim that linked proprietary modules are exempt from AGPL obligations.

- Resolve tenant identity from authenticated membership, never from an untrusted client field alone.
- Use tenant-scoped repositories and composite constraints for related records; assess PostgreSQL row-level security as defense in depth.
- Model plans and entitlements separately from workflow definitions; provide a local configuration for self-hosted deployments.
- Record usage in a durable ledger with a unique execution or metering event key, and enforce execution, concurrency and storage quotas before accepting work. Reconcile usage against actual execution records.
- Use fair scheduling and bounded per-tenant concurrency to prevent noisy neighbors. Scale API and workers independently after measurement.
- Keep payment-provider integration behind a billing adapter; verify signed billing events, handle duplicates and out-of-order delivery, and never trust frontend subscription state.
- Provide tenant export/deletion, backup and restoration procedures, retention limits, and observable operational health.
- Expose the applicable corresponding source to remote users of AGPL-covered deployments and track the deployed revision.

Billing and managed hosting are deferred until the core passes its acceptance criteria.

## Planned workspace

| Path | Responsibility |
| --- | --- |
| apps/web | React visual editor and execution history |
| apps/api | Authentication, authorization, workflow CRUD, triggers and events |
| apps/worker | Queue consumption and orchestration |
| packages/contracts | Shared schemas and versioned payload contracts |
| packages/engine | Graph validation and node execution |
| packages/nodes | Built-in adapters and registry |
| packages/database | Prisma schema and persistence helpers |

Use pnpm workspaces and Turborepo for task orchestration. Pin toolchain versions and commit a generated lockfile during scaffolding.

## Execution model

A workflow revision contains typed nodes, configuration and directed edges. An execution receives a revision identifier, organization identifier, trigger payload and execution identifier. Node handlers receive explicit input and a restricted execution context, returning output through named ports.

The first engine supports directed acyclic graphs only. Validate unique IDs, supported node types, valid endpoints and ports, a single manual trigger, reachability and absence of cycles before execution. Start with a single active predecessor per node; reject joins until their merge semantics are specified. Conditional nodes activate only the selected outgoing port. Unselected paths are skipped. Loops are deferred to a dedicated bounded iteration construct.

Run lifecycle: queued, running, succeeded, failed or cancelled. Node lifecycle: pending, running, succeeded, failed, skipped or cancelled. Persist state transitions, bounded outputs, sanitized errors and attempt metadata.

Manual execution initially uses local, in-memory orchestration for deterministic unit testing. Durable orchestration follows in the worker milestone. Introduce a transactional outbox or equivalent reconciliation to avoid losing jobs between database writes and queue publication. Queue delivery may repeat; use stable execution/attempt keys and transactional state checks. External side effects are not exactly-once unless the destination provides compatible idempotency semantics. Disable automatic retries for unsafe operations by default.

## Data model (planned)

- User, Organization, Membership
- Workflow and immutable WorkflowRevision
- Execution and NodeExecutionAttempt
- Credential reference and encrypted credential material
- Trigger configuration, OutboxEvent and AuditEvent
- Future: Plan, Entitlement, Subscription, UsageEvent and billing-event receipt

Use organization-scoped constraints and explicit access checks on all queries. Node execution results may include sensitive data; apply configurable retention and output limits.

## Integration contract

Each node defines a stable type, version, input/output schema, configuration schema, named ports, execution function, retry policy and permissions. The engine receives adapters through a registry; it never imports API or UI code.

## Acceptance criteria for MVP

A user can create a small workflow, save it, trigger it and inspect node results. Invalid graphs fail before side effects. Conditional paths run correctly; failed runs expose useful sanitized errors. A repeated delivery must not silently restart a completed run. Tenant A cannot read or execute tenant B's workflow.
