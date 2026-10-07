---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0001 — Every handler answers an outcome

## Context

Handlers retry, log, and translate what the layers inside them answered. Pudu has no exceptions to
catch, and a panic ends the program.

## Decision

Every handler, primary handler, and client helper answers `Outcome[T]`, a `Result[T, Failure]`
([[src/PuduLangHttpClient]]). A response with any status is an `Ok`; `Response.ensureSuccess` turns a
status outside 2xx into `Unsuccessful` when the caller asks.

## Consequences

- A resilience handler sees a 503 as a response and decides by predicate whether to retry it.
- Every refusal the package makes is a value with its cause.

## Rejected

- Treating non-success statuses as failures everywhere: handlers could no longer see the response.

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[decisions/_MOC]] · [[domain/Outcome]] · [[handoffs/2026-10-06-initial-package]]
