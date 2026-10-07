---
type: module
path: "@root/src/PuduLangHttpClient/Transport/Wire.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, leaf]
aliases: [PuduLangHttpClient.Transport.Wire]
---

# PuduLangHttpClient.Transport.Wire

## Purpose

One open connection, plain or secured, with every operation bounded by the request's deadline.

## Interface

### Signatures

```pudu
export type Link = Plain(Net.Connection) | Secured(Tls.Secured)

export type Fault = Stalled | Ended | Broken(HttpClient.Failure)

export fn budget(deadline: Option[Int]) -> Option[Int]

export fn dial(host: Str, port: Int, deadline: Option[Int]) -> Result[Link, Fault]

export fn secure(link: Link, host: Str, deadline: Option[Int]) -> Result[Link, Fault]

export fn write(link: &Link, payload: &Bytes, deadline: Option[Int]) -> Result[(), Fault]

export fn read(link: &Link, deadline: Option[Int]) -> Result[Option[Bytes], Fault]

export fn close(link: &Link) -> ()
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Domain/Expiry]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Exchange]], [[src/PuduLangHttpClient/Transport/Hop]], [[src/PuduLangHttpClient/Transport/Pool]].

## Algorithm

1. `budget` turns a deadline into milliseconds for one operation, or none once it has passed.
2. Socket errors become faults: a timeout is `Stalled`, a closed peer is `Ended`, and everything else a `Broken` failure.

## Negative Logic (Prohibited Paths)

- No operation waits without a budget.

## Edge Cases

- A plain connection being secured after its deadline is closed rather than leaked.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why bound each operation by the remaining deadline rather than polling for cancellation?
  **A:** On the current runtime a receive that times out closes the connection, so short polling reads are impossible; the deadline is the only interruption a blocking read honours. _Rejected:_ short reads in a loop checking the token.

## Referenced by

[[decisions/ADR-0004-deadline-cancellation]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Domain/Expiry]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Pool]] · [[src/PuduLangHttpClient/Transport/_MOC]]
