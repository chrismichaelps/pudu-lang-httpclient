---
type: module
path: "@root/src/PuduLangHttpClient/Options.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Options]
---

# PuduLangHttpClient.Options

## Purpose

Typed values a request carries through its handlers, keyed by name and written as text.

## Interface

### Signatures

```pudu
export type Options = { entries: Map[Str, Str] }

export type Key[T] = { name: Str, encode: fn(T) -> Str, decode: fn(Str) -> Option[T] }

export fn empty() -> Options

export fn key[T](name: Str, encode: fn(T) -> Str, decode: fn(Str) -> Option[T]) -> Key[T]

export fn textKey(name: Str) -> Key[Str]

export fn intKey(name: Str) -> Key[Int]

export fn boolKey(name: Str) -> Key[Bool]

export fn set[T](options: &Options, name: &Key[T], value: T) -> Options

export fn get[T](options: &Options, name: &Key[T]) -> Option[T]

export fn has[T](options: &Options, name: &Key[T]) -> Bool

export fn remove[T](options: &Options, name: &Key[T]) -> Options

export fn names(options: &Options) -> Array[Str]
```

### Linkage

- **Requires:** nothing.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Transport]].

## Algorithm

1. A `Key[T]` names an option and how its value is written and read.
2. `set` writes, `get` reads back, `remove` and `has` manage presence.

## Negative Logic (Prohibited Paths)

- A value that does not read back as the key's type is absent rather than an error.

## Edge Cases

- Two keys of different types under one name read each other's text.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why text-encoded values rather than any value?
  **A:** A request is a value that may be copied, compared, and logged; text keeps it comparable without erasing types. _Rejected:_ erased values (requests could no longer be compared).

## Referenced by

[[domain/Message]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Messages]]
