---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0003 — The factory builds, rotates, and closes pipelines

## Context

A pipeline that lives forever keeps resolving a host to the addresses it found first; one built per
request wastes connections. Pudu has no weak references or finalizers to learn when the last client
of a pipeline is gone.

## Decision

[[src/PuduLangHttpClient/Factory]] keeps one live pipeline per name for its handler lifetime (two
minutes by default), then builds the next. A retired pipeline's transport is closed once it has
outlived one more lifetime; a client still holding it keeps working, opening connections it closes.
`rotatingHandler` gives long-lived code a sender that always uses the current pipeline.

## Consequences

- Clients are cheap values made where needed.
- `withHandlerLifetime(INFINITE)` plus `withPooledConnectionLifetime` moves renewal into the pool.

## Rejected

- Never closing retired pipelines: their idle connections would leak.
- Closing them at once: clients made a moment before rotation would lose pooling immediately.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[domain/Lifetime]] · [[handoffs/2026-10-06-initial-package]] · [[subsystems/Factory]]
