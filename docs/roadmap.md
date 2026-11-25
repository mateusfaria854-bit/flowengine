# Development roadmap

This is an incremental plan, not a list of shipped features. Mark a milestone complete only after its acceptance criteria are verified.

| # | Milestone | Exit criteria |
| --- | --- | --- |
| 1 | Planning and architecture | Scope, graph semantics, security boundaries and roadmap documented |
| 2 | Monorepo and infrastructure | Reproducible install, build, lint, typecheck and local service setup |
| 3 | Authentication and organizations | Tenant isolation and access-control tests |
| 4 | Visual editor | Create, connect, validate and persist typed nodes |
| 5 | Execution engine | Deterministic traversal, branching and failure tests |
| 6 | Built-in nodes | Manual trigger, transform, condition and restricted HTTP integration |
| 7 | Queues and scheduling | Durable run lifecycle, reconciliation, cancellation and bounded retries |
| 8 | Integrations and credentials | Authorized webhooks and encrypted credential storage |
| 9 | Execution history and observability | Searchable sanitized history, metrics and live progress |
| 10 | Security and quality | Integration/E2E tests, abuse controls and documented threat mitigations |
| 11 | UX refinement | Accessible editor, responsive screens and useful examples |
| 12 | Deployment and documentation | Verified public demo, deployment guide, screenshots and real GIFs |

## MVP boundary

The MVP focuses on manual workflows, visual editing, a small node registry, durable execution and history. Webhooks and scheduling follow after the execution core is reliable.

Deferred: arbitrary user-written code, general loops, parallel joins, connector marketplace, AI nodes, billing and enterprise SSO. Revisit these only after the security and operational requirements are defined.

## Release discipline

Use small, reviewable commits. Keep main deployable once the runtime exists. Run relevant checks before merging runtime changes. Update documentation with implemented behavior, and label proposed features explicitly. Never publish real credentials or workplace inventory data in demonstrations.

## Hosted product track (after core milestones)

| Gate | Deliverable | Exit criteria |
| --- | --- | --- |
| A | Community release | Reproducible self-hosted installation, source availability and documented limitations |
| B | Cloud private pilot | Verified tenant isolation, quotas, usage accounting, encrypted secrets, monitoring, backup restoration and abuse controls |
| C | Paid beta | Validated customer demand, billing reconciliation, cancellation, data export/deletion, support procedures and legal/privacy review |
| D | General availability | Measured reliability, operating costs and support capacity; production readiness review |
| E | Enterprise | Reviewed alternative licensing, contracts and customer requirements |

These gates are separate from the original 12 engineering milestones. Do not start paid production hosting solely because the editor is complete. See [SaaS strategy](saas.md).
