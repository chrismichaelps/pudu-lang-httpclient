---
type: module
path: "@root/src/PuduLangHttpClient/Transport/Writer.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Transport.Writer]
---

# PuduLangHttpClient.Transport.Writer

## Purpose

A request written as the bytes that go on the wire.

## Interface

### Signatures

```pudu
export type Form = OriginForm | AbsoluteForm

export fn render(request: &Request.Request, version: &Http.Version, endpoint: &Uri.Endpoint, form: &Form, extra: &Array[(Str, Str)]) -> Bytes
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Domain/Uri]], [[src/PuduLangHttpClient/Request]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]].

## Algorithm

1. The request line names the origin-form target, or the absolute address through a plain proxy.
2. `host` comes first unless the request names one; the body's length is always computed from its bytes.

## Negative Logic (Prohibited Paths)

- A caller's `content-length` is never written; the computed one replaces it.

## Edge Cases

- A request framed by `transfer-encoding` states no length; a `POST`, `PUT`, or `PATCH` without a body states zero.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why replace a stated length?
  **A:** A length that disagrees with the body truncates it or leaves the server waiting; the bytes are known here. _Rejected:_ trusting the caller's header.

## Referenced by

[[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/_MOC]]
