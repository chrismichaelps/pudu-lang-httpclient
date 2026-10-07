---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0006 — Events travel through a mediator the package owns

## Context

Programs want to hear about requests and pipelines: to count, audit, or export them. Asking every
program to build a mediator, make notification kinds, and register handlers would make a messaging
library part of using an HTTP client.

## Decision

Programs register plain functions on [[src/PuduLangHttpClient/Events]] listeners and pass them to
`Factory.buildWith`. [[src/PuduLangHttpClient/Events/Bus]] builds a `pudu-lang-mediator` mediator
from them, one notification kind per event, and the factory publishes lifecycle events and adds the
bus's observer outermost to every pipeline when anyone listens to requests.

## Consequences

- Delivery is ordered and fans out to any number of listeners.
- No public signature names a mediator type.

## Rejected

- Exposing mediator kinds and registrations to programs.
- Calling listeners directly from the factory: every new event kind would grow the factory.

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[decisions/_MOC]] · [[domain/Events]] · [[handoffs/2026-10-06-initial-package]] · [[seams/Listeners]] · [[src/PuduLangHttpClient/Events]]
