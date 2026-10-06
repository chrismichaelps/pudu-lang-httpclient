---
type: module
path: "@root/src/PuduLangHttpClient/Cookies.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Cookies]
---

# PuduLangHttpClient.Cookies

## Purpose

A jar of cookies shared by the requests of one transport, saved and restored as JSON.

## Interface

### Signatures

```pudu
export type Jar = { held: Shared.Shared[Array[Cookie.Stored]] }

export fn jar() -> Jar

export fn store(target: &Jar, uri: Str, values: &Array[Str]) -> ()

export fn add(target: &Jar, uri: Str, name: Str, value: Str) -> ()

export fn cookiesFor(source: &Jar, uri: Str) -> Array[Cookie.Stored]

export fn header(source: &Jar, uri: Str) -> Option[Str]

export fn cookies(source: &Jar) -> Array[Cookie.Stored]

export fn clear(target: &Jar) -> ()

export fn clearSession(target: &Jar) -> ()

export fn save(source: &Jar) -> Str

export fn restore(target: &Jar, saved: Str) -> HttpClient.Outcome[()]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Domain/Cookie]], [[src/PuduLangHttpClient/Domain/Uri]], [[src/PuduLangHttpClient/Utils/Shared]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]].

## Algorithm

1. `store` parses `set-cookie` values for the response's host and path at the current wall-clock time.
2. `header` renders the cookies that apply to an address.
3. `save` writes the persistent cookies; `restore` stores them again.

## Negative Logic (Prohibited Paths)

- Session cookies are never saved.

## Edge Cases

- An address that is not http or https matches no cookie.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why off by default in the transport?
  **A:** A pooled transport shares its jar between every client of its name; cookies from one caller would reach another, and a rotated pipeline loses them. _Rejected:_ cookies on by default.

## Referenced by

[[decisions/ADR-0007-secure-defaults]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Domain/Cookie]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Utils/Shared]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Transport]]
