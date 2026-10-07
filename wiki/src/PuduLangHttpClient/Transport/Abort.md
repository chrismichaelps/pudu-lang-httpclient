---
type: module
path: "@root/src/PuduLangHttpClient/Transport/Abort.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Transport.Abort]
---

# PuduLangHttpClient.Transport.Abort

## Purpose

A connection closed when its request is cancelled mid-exchange, so a read or write blocked on it wakes at once.

## Interface

### Signatures

```pudu
export type Stage = Running | Finished | Aborted

export type Guard = { stage: Shared.Shared[Stage] }

export fn watch(link: &Wire.Link, token: &Cancel.Token) -> Guard

export fn finish(guard: &Guard) -> Bool
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Transport/Wire]], [[src/PuduLangHttpClient/Utils/Shared]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport/Hop]].

## Algorithm

1. `watch` starts a watcher that looks for an explicit cancellation every 10 ms while the exchange runs and closes the link when it finds one.
2. `finish` ends the exchange and answers whether the watcher aborted it.
3. The stage moves from running to finished or to aborted exactly once, under a lock.

## Negative Logic (Prohibited Paths)

- Once `finish` answers, the watcher never touches the link again, so a connection returned to the pool is never closed by a stale watcher.
- A deadline is not the watcher's to act on: the socket operations already wait at most what remains of it.

## Edge Cases

- A watcher outlives its exchange by at most one poll and then ends without acting.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why close the socket rather than poll it with short reads?
  **A:** On the runtime a receive that times out closes the connection, while closing a socket from another thread wakes a blocked receive at once; closing is the one interruption a blocked read honours. _Rejected:_ polling reads (they destroy the connection they poll).
- **Q:** Why a thread per exchange?
  **A:** Threads are lightweight on the runtime, and a loopback benchmark of 2000 keep-alive requests measured no difference with and without the watcher. _Rejected:_ one watcher thread per transport tracking every request (shared state on every request's path).

## Referenced by

[[CHANGELOG]] · [[decisions/ADR-0004-deadline-cancellation]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Transport/_MOC]] · [[src/PuduLangHttpClient/Utils/Shared]]
