---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0004 — Cancellation is cooperative and bounded by deadlines

## Context

On the 0.1.3 runtime a socket receive that times out closes the connection, so a request cannot poll
its token with short reads; and a thread cannot be interrupted.

## Decision

Every socket operation waits at most what remains of the request's deadline
([[src/PuduLangHttpClient/Transport/Wire]]); the token is checked between phases and while waiting
for a pooled connection. A client links the caller's token, its pending token, and its timeout
([[src/PuduLangHttpClient/Utils/Tokens]]).

## Consequences

- Timeouts are exact; an explicit cancellation takes effect at the next phase or the deadline.
- `cancelPending` stops requests waiting for a connection or between phases at once.

## Rejected

- A watcher thread per request closing its socket on cancellation: a thread for every request.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[handoffs/2026-10-06-initial-package]]
