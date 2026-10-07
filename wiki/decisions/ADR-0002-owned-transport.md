---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0002 — The package owns its transport

## Context

`Std.Http.Client` sends `connection: close` and opens a new connection, and for https a new
handshake, for every request. A factory's value is reusing connections per name, bounding them per
server, and renewing them by lifetime.

## Decision

[[src/PuduLangHttpClient/Transport]] speaks HTTP/1.1 over `Std.Net` and `Std.Tls` itself: it pools
connections per route ([[src/PuduLangHttpClient/Transport/Pool]]), reads each response's framing
exactly ([[src/PuduLangHttpClient/Transport/Exchange]]), and reuses the parsers and safety checks of
`Std.Http`, `Std.Http.Message`, and `Std.Http.Safe`.

## Consequences

- Keep-alive, connection lifetimes, idle timeouts, per-server limits, proxies with tunnels, and
  streamed bodies are all possible.
- The transport speaks HTTP/1.0 and 1.1; HTTP/2 requests are downgraded or refused by policy.
- Client certificates are not offered: `Std.Tls` in 0.1.3 has no way to present one.

## Rejected

- `Std.Http.Client.send` per request: no reuse, no limits, no streaming.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[domain/Connection]] · [[handoffs/2026-10-06-initial-package]] · [[src/PuduLangHttpClient/Transport]] · [[subsystems/Transport]]
