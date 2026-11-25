# Security design

This document describes intended controls, not an audit or a claim that the controls already exist.

## Trust boundaries

Workflow definitions, node configuration, webhook payloads and external responses are untrusted. Treat the API and worker as separate privilege boundaries. Enforce authorization independently of identifiers supplied by clients.

- Validate input and cap graph size, execution time, concurrency, queue depth and payload sizes.
- Encrypt credential material with authenticated encryption and an externally managed key; store only credential references in workflow definitions.
- Never return credential plaintext through API responses or include it in logs.
- For HTTP nodes, restrict destinations; block loopback, private, link-local and metadata networks, including IPv6 and redirects. Validate DNS resolution and the actual connected address, and apply network-level egress restrictions. Bound response size and time.
- Authenticate webhook endpoints using per-workflow secrets or verified signatures; rate-limit and prevent replay where the integration supports it.
- Enforce tenant-scoped authorization for workflows, credentials, executions and event subscriptions.
- Treat execution inputs and outputs as potentially sensitive; redact, restrict access and define retention.
- Defer arbitrary user code until a separately isolated, resource-constrained runtime is designed. Node.js vm alone is not an adequate security boundary.
- Require explicit approval and authorized integration access before automating real inventory adjustments or other consequential writes.

## Reporting

Do not publish secrets, exploit payloads containing personal data or sensitive production logs in public issues. Use GitHub private vulnerability reporting if enabled; otherwise contact the repository maintainer privately before public disclosure.
