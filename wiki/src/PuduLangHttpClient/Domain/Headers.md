---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Headers.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Headers]
---

# PuduLangHttpClient.Domain.Headers

## Purpose

Header lists compared without regard to case: lookups, sets, removals, defaults merged under a message's own headers, redaction, the split between message and content headers, and writability checks.

## Interface

### Signatures

```pudu
export const REDACTED: Str

export fn get(headers: &Array[(Str, Str)], name: Str) -> Option[Str]

export fn values(headers: &Array[(Str, Str)], name: Str) -> Array[Str]

export fn has(headers: &Array[(Str, Str)], name: Str) -> Bool

export fn add(headers: &Array[(Str, Str)], name: Str, value: Str) -> Array[(Str, Str)]

export fn set(headers: &Array[(Str, Str)], name: Str, value: Str) -> Array[(Str, Str)]

export fn remove(headers: &Array[(Str, Str)], name: Str) -> Array[(Str, Str)]

export fn removeAll(headers: &Array[(Str, Str)], names: &Set[Str]) -> Array[(Str, Str)]

export fn merged(defaults: &Array[(Str, Str)], own: &Array[(Str, Str)]) -> Array[(Str, Str)]

export fn redacted(headers: &Array[(Str, Str)], redact: fn(Str) -> Bool) -> Array[(Str, Str)]

export fn isContentHeader(name: Str) -> Bool

export fn splitContent(headers: &Array[(Str, Str)]) -> (Array[(Str, Str)], Array[(Str, Str)])

export fn unwritable(headers: &Array[(Str, Str)]) -> Option[Str]

export fn lists(value: Str, token: Str) -> Bool
```

### Linkage

- **Requires:** the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Domain/Persistence]], [[src/PuduLangHttpClient/Handlers/Authorization]], [[src/PuduLangHttpClient/Handlers/Logging]], [[src/PuduLangHttpClient/Handlers/Propagation]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Writer]].

## Algorithm

1. Names are compared lower-cased; values are kept as written.
2. `merged` keeps a default only when the message lacks its name, defaults first.
3. `unwritable` refuses an empty name, a colon in a name, and anything `Std.Http.Safe.headerWritable` refuses.

## Negative Logic (Prohibited Paths)

- Headers are never reordered except by `set`, which moves the replaced name to the end.

## Edge Cases

- `lists` matches a whole comma-separated token, so `closed` does not list `close`.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why keep headers as an ordered list rather than a map?
  **A:** Order and repetition are meaningful on the wire (several `set-cookie` values, several `accept` entries); a map would merge or reorder them. _Rejected:_ a map from name to value.

## Referenced by

[[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Domain/Persistence]] · [[src/PuduLangHttpClient/Domain/_MOC]] · [[src/PuduLangHttpClient/Handlers/Authorization]] · [[src/PuduLangHttpClient/Handlers/Logging]] · [[src/PuduLangHttpClient/Handlers/Propagation]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Writer]]
