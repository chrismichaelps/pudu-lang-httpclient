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
for a pooled connection. While an exchange runs, a watcher closes its connection as soon as the token
is cancelled by request ([[src/PuduLangHttpClient/Transport/Abort]]), which wakes the blocked read or
write at once. A client links the caller's token, its pending token, and its timeout
([[src/PuduLangHttpClient/Utils/Tokens]]).

## Consequences

- Timeouts are exact, and an explicit cancellation stops a request mid-read within one poll.
- `cancelPending` stops every request in flight, wherever it is.
- A cancelled request's connection is closed rather than pooled.

## Rejected

- Short polling reads: on the runtime a receive that times out closes the connection.
- Cancellation only between phases: a request blocked on a slow server would wait for its deadline.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[handoffs/2026-10-06-initial-package]]
