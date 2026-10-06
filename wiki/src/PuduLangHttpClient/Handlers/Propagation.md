---
type: module
path: "@root/src/PuduLangHttpClient/Handlers/Propagation.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.4
depth_status: SHALLOW
tags: [module, leaf]
aliases: [PuduLangHttpClient.Handlers.Propagation]
---

# PuduLangHttpClient.Handlers.Propagation

## Purpose

Headers of the operation in progress copied onto outgoing requests, under the same or another name.

## Interface

### Signatures

```pudu
export type Source = fn() -> Array[(Str, Str)]

export fn handler(names: &Array[Str], source: Source) -> Handler.Handler

export fn renaming(mappings: &Array[(Str, Str)], source: Source) -> Handler.Handler
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], the standard library.
- **Consumed by:** the program.

## Algorithm

1. The source is read once per request; each mapped header is copied with every value when the request lacks it.

## Negative Logic (Prohibited Paths)

- A request's own header always wins.

## Edge Cases

- A header the source lacks is not sent.

## Depth

DEPTH 0.4 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a source function rather than ambient state?
  **A:** Pudu has no context that follows a call across threads; the program decides where the current operation's headers live. _Rejected:_ a global cell shared by every request.

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/_MOC]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[subsystems/Handlers]]
