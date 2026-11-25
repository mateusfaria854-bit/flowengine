# FlowEngine

**Build workflows. Connect systems. Automate everything.**

FlowEngine is an open-source workflow automation platform designed around a visual editor, an event-driven execution engine, asynchronous processing and extensible integrations.

> **Status:** Early development. This repository currently contains the design and roadmap; the runtime and visual editor are not implemented yet.

## Product vision

Build reusable automations by connecting triggers, actions and conditions on a visual canvas. Inspect execution results, handle failures and integrate external services without writing a complete application.

## Planned capabilities

- Visual workflow editor with typed nodes and edges
- Manual, scheduled and webhook triggers
- HTTP requests, data transformations and conditional routing
- Background execution, bounded retries and execution history
- Organization-scoped workflows and encrypted credentials
- Real-time execution updates and extensible integrations

## Architecture

```mermaid
flowchart LR
  Web[React web app] --> API[NestJS API]
  API --> DB[(PostgreSQL)]
  API --> Queue[(Redis / BullMQ)]
  Queue --> Worker[Execution worker]
  Worker --> Engine[Workflow engine]
  Worker --> DB
  Engine --> Nodes[Node adapters]
  Worker --> Events[Execution events]
  Events --> API
  API --> Web
```

The diagram describes the target architecture, not deployed services.

| Layer | Planned technologies |
| --- | --- |
| Web | React, TypeScript, React Flow, Tailwind CSS |
| API | NestJS, PostgreSQL, Prisma |
| Execution | TypeScript, Redis, BullMQ |
| Delivery | Docker, GitHub Actions, Railway |
| Quality | Vitest, Playwright, OpenTelemetry |

## Documentation

- [Architecture and execution semantics](docs/architecture.md)
- [Development roadmap](docs/roadmap.md)
- [Security design](docs/security.md)
- [Contributing](CONTRIBUTING.md)

## Getting started

Executable installation instructions will be published once the initial application scaffold is available. No running demo or production readiness is claimed at this stage.

## License

[MIT](LICENSE).
