---
type: module
path: "@root/src/PuduLangHttpClient/Utils/Tokens.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Utils.Tokens]
---

# PuduLangHttpClient.Utils.Tokens

## Purpose

Cancellation tokens that fire when any of several parents fires, or after a timeout of their own.

## Interface

### Signatures

```pudu
export fn linked(parents: &Array[Cancel.Token], millis: Int) -> Cancel.Token
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Constants/Defaults]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]].

## Algorithm

1. `linked` gathers each parent's ancestors and own flag into the new token's ancestors.
2. The deadline is the earliest of the parents' deadlines and `millis` from now.

## Negative Logic (Prohibited Paths)

- Cancelling the linked token never cancels a parent.

## Edge Cases

- `INFINITE` with parents that have no deadline leaves a token without one; a negative timeout fires at once.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why build the token record directly instead of chaining `Cancel.child`?
  **A:** A child has one parent; a request must stop for the caller's token, the client's pending token, and the client's timeout at once, and the standard token's lineage is an array of flags meant to be shared this way. _Rejected:_ a watcher thread per request (a thread for every request in flight).

## Referenced by

[[decisions/ADR-0004-deadline-cancellation]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Utils/_MOC]]
