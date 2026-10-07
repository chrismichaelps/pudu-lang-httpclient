---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Expiry.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Expiry]
---

# PuduLangHttpClient.Domain.Expiry

## Purpose

Whether a lifetime or idle limit has run out, what remains of a deadline, and the earlier of two deadlines.

## Interface

### Signatures

```pudu
export fn elapsed(since: Int, lifetime: Int, now: Int) -> Bool

export fn remaining(deadline: Option[Int], now: Int) -> Option[Int]

export fn earliest(left: Option[Int], right: Option[Int]) -> Option[Int]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Constants/Defaults]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Domain/Persistence]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Wire]].

## Algorithm

1. `elapsed` is true once `now - since` reaches the lifetime; `INFINITE` never runs out.

## Negative Logic (Prohibited Paths)

- No clock is read here; every function takes `now`.

## Edge Cases

- A lifetime of 0 has already run out.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why take `now` as a parameter?
  **A:** Pure functions are tested at exact boundaries and mutation-tested; the effect of reading the clock stays in the callers. _Rejected:_ reading the clock inside.

## Referenced by

[[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Domain/Persistence]] · [[src/PuduLangHttpClient/Domain/_MOC]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Wire]]
