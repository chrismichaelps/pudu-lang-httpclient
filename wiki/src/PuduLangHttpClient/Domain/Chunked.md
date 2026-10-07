---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Chunked.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.9
depth_status: DEEP
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Chunked]
---

# PuduLangHttpClient.Domain.Chunked

## Purpose

A chunked body decoded incrementally as its bytes arrive, with trailers and leftover bytes kept, bounded by a limit on decoded bytes.

## Interface

### Signatures

```pudu
export type Phase = Size | Data(Int) | DataEnd | Trailer | Done

export type Decoder = { phase: Phase, pending: Bytes, decoded: Int, trailers: Array[(Str, Str)] }

export type Problem = Malformed(Str) | Exceeded

export fn decoder() -> Decoder

export fn isDone(state: &Decoder) -> Bool

export fn leftover(state: &Decoder) -> Bytes

export fn feed(state: &Decoder, input: &Bytes, limit: Int) -> Result[(Decoder, Bytes), Problem]
```

### Linkage

- **Requires:** the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport/Exchange]].

## Algorithm

1. The decoder is a phase (`Size`, `Data(n)`, `DataEnd`, `Trailer`, `Done`) plus the bytes not yet consumed.
2. `feed` appends the input and advances through as many phases as the bytes allow, answering the body bytes it completed.
3. A framing line is read up to its line break; sizes are hexadecimal with any extension after `;` ignored.

## Negative Logic (Prohibited Paths)

- A framing line longer than 8192 bytes, a size of more than 15 digits, a chunk not followed by a line break, or a nameless trailer is `Malformed`.
- Decoded bytes past the limit are `Exceeded` before they are kept.

## Edge Cases

- Bytes after the terminating chunk are leftover; a connection with leftover bytes is not reused.

## Depth

DEPTH 0.9 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why incremental rather than decoding the whole body at the end?
  **A:** A streamed body is handed to a receiver as each chunk arrives; and the limit is enforced while reading instead of after holding everything. _Rejected:_ `Std.Http.Message.decodeChunkedBytes` after buffering (no streaming, late limit).

## Referenced by

[[src/PuduLangHttpClient/Domain/_MOC]] · [[src/PuduLangHttpClient/Transport/Exchange]]
