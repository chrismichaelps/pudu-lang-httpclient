---
type: module
path: "@root/src/PuduLangHttpClient/Content.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, leaf]
aliases: [PuduLangHttpClient.Content]
---

# PuduLangHttpClient.Content

## Purpose

Message bodies and the headers that describe them: text, bytes, JSON documents and derived values, forms, and multipart bodies, plus readers for each.

## Interface

### Signatures

```pudu
export type Producer = fn() -> Option[Bytes]

export type Content = { payload: Bytes, headers: Array[(Str, Str)], stream: Option[Producer] }

export type Part = { name: Str, filename: Option[Str], mediaType: Option[Str], payload: Bytes }

export fn empty() -> Content

export fn text(body: Str, mediaType: Str) -> Content

export fn plain(body: Str) -> Content

export fn bytes(payload: &Bytes, mediaType: Str) -> Content

export fn octets(payload: &Bytes) -> Content

export fn json(value: &Json.Json) -> Content

export fn jsonOf[T: Json.Encode](value: &T) -> Content

export fn form(fields: &Array[(Str, Str)]) -> Content

export fn streamed(mediaType: Str, producer: Producer) -> Content

export fn isStreamed(content: &Content) -> Bool

export fn field(name: Str, value: Str) -> Part

export fn file(name: Str, filename: Str, mediaType: Str, payload: &Bytes) -> Part

export fn multipart(parts: &Array[Part]) -> Content

export fn multipartWith(boundary: Str, parts: &Array[Part]) -> Content

export fn withHeader(content: &Content, name: Str, value: Str) -> Content

export fn mediaTypeOf(content: &Content) -> Option[Str]

export fn length(content: &Content) -> Int

export fn readText(content: &Content) -> HttpClient.Outcome[Str]

export fn readJson(content: &Content) -> HttpClient.Outcome[Json.Json]

export fn readAs[T: Json.Decode](content: &Content) -> HttpClient.Outcome[T]

export fn readForm(content: &Content) -> HttpClient.Outcome[Array[(Str, Str)]]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Constants/Messages]], [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Utils/Template]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Client/Json]], [[src/PuduLangHttpClient/Cookies]], [[src/PuduLangHttpClient/Endpoint]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Stub]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Exchange]], [[src/PuduLangHttpClient/Transport/Hop]].

## Algorithm

1. A body is its bytes and its content headers.
2. `jsonOf` writes any `Json.Encode` value; `readAs` reads any `Json.Decode` value.
3. `multipart` frames parts with a random boundary; `multipartWith` takes one.

## Negative Logic (Prohibited Paths)

- A body that is not UTF-8 is never read as text; it is `Unreadable`.

## Edge Cases

- A file name's quotes and line breaks are escaped or removed in multipart headers.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why bytes plus headers rather than a type per kind of body?
  **A:** Every body goes on the wire as bytes; constructors decide the bytes and the headers once, and readers decide how to read them. _Rejected:_ a sum type of body kinds.

## Referenced by

[[domain/Message]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Client/Json]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Cookies]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Endpoint]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Stub]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Utils/Template]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Messages]]
