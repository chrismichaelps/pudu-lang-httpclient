---
type: module
path: "@root/src/PuduLangHttpClient/Factory/Entry.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Factory.Entry]
---

# PuduLangHttpClient.Factory.Entry

## Purpose

One built handler pipeline: its layers composed around a new primary handler, its transport, and when it was built.

## Interface

### Signatures

```pudu
export type Entry = {
  name: Str,
  generation: Int,
  send: Handler.Send,
  transport: Option[Transport.Transport],
  created: Int,
  lifetime: Int,
  expiredAt: Option[Int]
}

export fn build(builder: &Builder.Builder, generation: Int) -> Entry

export fn layers(builder: &Builder.Builder) -> Array[Handler.Handler]

export fn close(entry: &Entry) -> ()

export fn transportOptions(builder: &Builder.Builder, options: Transport.Options) -> Transport.Options
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Factory/Builder]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Transport]].
- **Consumed by:** [[src/PuduLangHttpClient/Factory]].

## Algorithm

1. Layers are the observers' outer handlers, the added handlers as rearranged, then the observers' inner handlers in reverse.
2. A pooled primary handler gets a new transport with the builder's edits.

## Negative Logic (Prohibited Paths)

- Handler factories run once per entry, never per request.

## Edge Cases

- A primary handler of the program's own has no transport to close.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why observers both outside and inside the handlers?
  **A:** Outside, they see what the caller sent and got; inside, they see each attempt on the wire after retries and rewrites. _Rejected:_ one position per observer.

## Referenced by

[[architecture/_MOC]] · [[domain/Pipeline]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Factory/_MOC]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Transport]] · [[subsystems/Factory]]
