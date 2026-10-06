---
type: module
path: "@root/src/PuduLangHttpClient/Utils/Template.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.4
depth_status: SHALLOW
tags: [module, leaf]
aliases: [PuduLangHttpClient.Utils.Template]
---

# PuduLangHttpClient.Utils.Template

## Purpose

Fills numbered `<n>` slots in message text in one pass.

## Interface

### Signatures

```pudu
export fn fill(template: Str, values: &Array[Str]) -> Str
```

### Linkage

- **Requires:** the standard library.
- **Consumed by:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Endpoint]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Exchange]].

## Algorithm

1. Scans for `<`, reads a number up to `>`, and substitutes the value at that position, counting from 1.

## Negative Logic (Prohibited Paths)

- Text taken from a value is never filled again.

## Edge Cases

- A slot without a value, or a `<` that is not a slot, is kept as written.

## Depth

DEPTH 0.4 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why numbered slots rather than interpolation?
  **A:** Templates are constants, and constants cannot interpolate runtime values. _Rejected:_ building sentences inline.

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Endpoint]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Utils/_MOC]]
