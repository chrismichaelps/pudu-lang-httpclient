---
type: module
path: "@root/src/PuduLangHttpClient/Client/Json.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Client.Json]
---

# PuduLangHttpClient.Client.Json

## Purpose

Requests and responses whose bodies are JSON, read and written through derived implementations.

## Interface

### Signatures

```pudu
export fn getJson(client: &Client.Client, uri: Str) -> HttpClient.Outcome[Json.Json]

export fn getFromJson[T: Json.Decode](client: &Client.Client, uri: Str) -> HttpClient.Outcome[T]

export fn deleteFromJson[T: Json.Decode](client: &Client.Client, uri: Str) -> HttpClient.Outcome[T]

export fn postAsJson[T: Json.Encode](client: &Client.Client, uri: Str, value: &T) -> HttpClient.Outcome[Response.Response]

export fn putAsJson[T: Json.Encode](client: &Client.Client, uri: Str, value: &T) -> HttpClient.Outcome[Response.Response]

export fn patchAsJson[T: Json.Encode](client: &Client.Client, uri: Str, value: &T) -> HttpClient.Outcome[Response.Response]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], the standard library.
- **Consumed by:** the program.

## Algorithm

1. Every helper asks for `application/json`.
2. Readers check success first, then decode.

## Negative Logic (Prohibited Paths)

- A failed status is never decoded.

## Edge Cases

- A body that is not JSON is `Unreadable`.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why constrain on `Json.Encode` and `Json.Decode` rather than taking functions?
  **A:** A type that derives the traits needs no hand-written encoder; the constraint states exactly what the helper uses. _Rejected:_ encoder and decoder arguments.

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Client/_MOC]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]]
