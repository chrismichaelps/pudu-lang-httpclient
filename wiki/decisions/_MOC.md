---
type: moc
tags: [moc, adr]
---

# Decisions

- [[decisions/ADR-0001-outcomes-not-exceptions]] — every handler answers an outcome value.
- [[decisions/ADR-0002-owned-transport]] — the package owns its pooled keep-alive transport.
- [[decisions/ADR-0003-factory-owned-pipelines]] — pipelines are built, rotated, and closed by the factory.
- [[decisions/ADR-0004-deadline-cancellation]] — cancellation is cooperative and bounded by deadlines.
- [[decisions/ADR-0005-integrations-at-the-edge]] — logging and resilience live under `Handlers/`.
- [[decisions/ADR-0006-internal-event-bus]] — events travel through a mediator the package owns.
- [[decisions/ADR-0007-secure-defaults]] — credentials, cookies, redirects, and logs default to safe.

## Referenced by

[[00-INDEX]]
