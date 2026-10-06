---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Persistence.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Persistence]
---

# PuduLangHttpClient.Domain.Persistence

## Purpose

Which connections stay open after an exchange, and which pooled connections may still be reused.

## Interface

### Signatures

```pudu
export type Idle[C] = { link: C, created: Int, lastUsed: Int }

export type Limits = { lifetime: Int, idleTimeout: Int }

export fn keepsAlive(version: Str, requestHeaders: &Array[(Str, Str)], responseHeaders: &Array[(Str, Str)], framing: &Message.Framing) -> Bool

export fn usable[C](idle: &Idle[C], limits: &Limits, now: Int) -> Bool

export fn sweep[C](idle: &Array[Idle[C]], limits: &Limits, now: Int) -> (Array[Idle[C]], Array[Idle[C]])
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Domain/Expiry]], [[src/PuduLangHttpClient/Domain/Headers]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Exchange]], [[src/PuduLangHttpClient/Transport/Pool]].

## Algorithm

1. `keepsAlive` refuses when either side listed `close`, the body ended with the connection, or an HTTP/1.0 response did not ask to keep it alive.
2. `usable` checks the connection's lifetime from creation and its idle limit from last use.
3. `sweep` splits idle connections into usable and expired.

## Negative Logic (Prohibited Paths)

- No socket is touched here.

## Edge Cases

- Infinite limits keep a connection usable forever.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why decide reuse after every exchange rather than trusting the protocol version?
  **A:** A server may close any connection, and a body framed by closing leaves nothing to reuse; reusing it would read the next response from a dead socket. _Rejected:_ reusing every HTTP/1.1 connection.

## Referenced by

[[domain/Connection]] · [[src/PuduLangHttpClient/Domain/Expiry]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Domain/_MOC]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Pool]]
