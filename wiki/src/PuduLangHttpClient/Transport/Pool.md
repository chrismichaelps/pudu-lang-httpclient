---
type: module
path: "@root/src/PuduLangHttpClient/Transport/Pool.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangHttpClient.Transport.Pool]
---

# PuduLangHttpClient.Transport.Pool

## Purpose

Idle connections kept per origin and handed out again, within a per-origin limit, swept by lifetime and idle time.

## Interface

### Signatures

```pudu
export type Statistics = { opened: Int, reused: Int, closed: Int, active: Int, idle: Int } derives Json.Encode

export type Lease = Reused(Persistence.Idle[Wire.Link]) | Fresh

export type Pool = { state: Shared.Shared[State], limits: Persistence.Limits, maxPerServer: Int }

export fn create(limits: Persistence.Limits, maxPerServer: Int) -> Pool

export fn acquire(pool: &Pool, key: Str, token: &Cancel.Token, deadline: Option[Int]) -> Result[Lease, Wire.Fault]

export fn release(pool: &Pool, key: Str, link: Wire.Link, created: Int, reusable: Bool) -> ()

export fn forfeit(pool: &Pool, key: Str) -> ()

export fn statistics(pool: &Pool) -> Statistics

export fn close(pool: &Pool) -> ()

export fn isClosed(pool: &Pool) -> Bool
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Domain/Persistence]], [[src/PuduLangHttpClient/Transport/Wire]], [[src/PuduLangHttpClient/Utils/Shared]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Hop]].

## Algorithm

1. `acquire` sweeps every origin's idle connections, reuses the most recently released one, or reserves a new connection while the origin is under its limit.
2. At the limit, the request waits in short slices until a release, its token fires, or its deadline passes.
3. `release` keeps a reusable connection idle, or closes it; `forfeit` gives back a reservation that could not connect.

## Negative Logic (Prohibited Paths)

- Sockets are closed outside the lock.
- A closed pool keeps no connection again.

## Edge Cases

- Counters report connections opened, reused, closed, in use, and idle.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why reuse the most recently released connection?
  **A:** It is the one least likely to have been closed by the server for idleness. _Rejected:_ first in, first out.
- **Q:** Why poll while waiting for a free connection?
  **A:** The standard library offers no wait with a deadline and a token on one condition; a short sleep that honours both is simpler and only costs anything when the origin is saturated. _Rejected:_ a semaphore wait (cannot honour the deadline).

## Referenced by

[[decisions/ADR-0002-owned-transport]] · [[domain/Connection]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Domain/Persistence]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Transport/_MOC]] · [[src/PuduLangHttpClient/Utils/Shared]]
