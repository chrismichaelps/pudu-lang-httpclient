---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0005 — Logging and resilience live under `Handlers/`

## Context

Structured logging and resilience policies are the handlers most programs add first, and two
packages already provide them.

## Decision

[[src/PuduLangHttpClient/Handlers/Logging]] uses `pudu-lang-log` and
[[src/PuduLangHttpClient/Handlers/Resilience]] uses `pudu-lang-resilience`; nothing else in the
package imports them ([[subsystems/Handlers]]).

## Consequences

- The client, factory, and transport read on their own.
- A program that never imports the handlers never touches the two packages.

## Rejected

- Hand-written retries and logging inside the transport: duplicates maintained packages.

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[decisions/_MOC]] · [[handoffs/2026-10-06-initial-package]] · [[subsystems/Handlers]]
