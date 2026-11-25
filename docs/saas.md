# SaaS product strategy

Status: proposal. No hosted service, pricing or customer commitments exist yet.

## Positioning

FlowEngine Community is an AGPL-licensed self-hosted automation platform. FlowEngine Cloud is a proposed paid managed service built on the same core. Enterprise alternative licensing may be evaluated once the necessary rights and contracts are established.

Start with a focused audience: small teams that rely on spreadsheets, repetitive administrative tasks and authorized business-system integrations. Validate demand in Brazil before expanding, without preventing international adoption.

## First product experiments

- Import synthetic CSV/Excel data, validate rows, transform fields and send approved records to a sandbox API.
- Trigger a workflow from an authorized webhook and inspect its execution history.
- Provide templates, clear error handling and usage visibility for non-specialists.

Do not integrate real workplace systems or data without explicit authorization and appropriate safeguards.

## Business model hypotheses

Managed hosting, operational support, backup and capacity tiers are potential paid benefits. Evaluate execution-based quotas together with compute time, outbound traffic, storage, retention and concurrency. Establish pricing only after customer interviews, load testing and cost measurements. Monitor active organizations, successful executions, retention, gross margin and support burden.

## Release requirements

The hosted pilot requires tenant isolation, encrypted secrets, abuse prevention, resource metering, quota enforcement, audit events, tested recovery and source availability. Before taking payments, validate billing, cancellation, refunds, data export/deletion, terms, privacy, applicable tax obligations and dependency licensing with qualified specialists.

The Community and Cloud experience should remain usable without proprietary payment infrastructure in the execution core. See [architecture](architecture.md), [security](security.md) and [roadmap](roadmap.md).
