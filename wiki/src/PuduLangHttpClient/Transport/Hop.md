---
type: module
path: "@root/src/PuduLangHttpClient/Transport/Hop.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, leaf]
aliases: [PuduLangHttpClient.Transport.Hop]
---

# PuduLangHttpClient.Transport.Hop

## Purpose

One request sent over a pooled connection, retried once on a new connection when a reused one turns out to have been closed while idle.

## Interface

### Signatures

```pudu
export type Sending = {
  written: Bytes,
  method: Http.Method,
  headers: Array[(Str, Str)],
  reading: Exchange.Reading,
  connectTimeout: Int,
  deadline: Option[Int],
  receiver: Option[Request.Receiver],
  following: Bool,
  upload: Option[Content.Producer]
}

export fn send(pool: &Pool.Pool, route: &Connect.Route, sending: &Sending, token: &Cancel.Token) -> Result[Exchange.Exchanged, Wire.Fault]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Exchange]], [[src/PuduLangHttpClient/Transport/Pool]], [[src/PuduLangHttpClient/Transport/Wire]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]].

## Algorithm

1. A lease is reused or opened; the exchange runs; the connection is released by what the exchange said.
2. A failure on a reused connection before any response byte arrived, from a closed or reset socket, is retried once on a fresh connection.

## Negative Logic (Prohibited Paths)

- A timeout is never retried here; nor is a failure after any part of the response arrived.

## Edge Cases

- A connection that could not be opened forfeits its reservation.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why retry regardless of method?
  **A:** A server that closed an idle connection never received the request; repeating it cannot repeat an effect. _Rejected:_ retrying only idempotent methods (fails requests that were never seen).

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Pool]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Transport/_MOC]]
