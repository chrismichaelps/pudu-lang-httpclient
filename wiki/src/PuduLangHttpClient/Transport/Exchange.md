---
type: module
path: "@root/src/PuduLangHttpClient/Transport/Exchange.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangHttpClient.Transport.Exchange]
---

# PuduLangHttpClient.Transport.Exchange

## Purpose

One request written and one response read over a connection: interim responses skipped, the head parsed, and the body read by its framing, buffered or streamed.

## Interface

### Signatures

```pudu
export type Reading = { headersLimit: Int, bodyLimit: Int }

export type Head = { version: Str, status: Http.Status, headers: Array[(Str, Str)] }

export type Exchanged = { head: Head, body: Bytes, trailers: Array[(Str, Str)], reusable: Bool }

export type Trouble = { fault: Wire.Fault, answered: Bool }

export fn exchange(
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Constants/Messages]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Domain/Chunked]], [[src/PuduLangHttpClient/Domain/Persistence]], [[src/PuduLangHttpClient/Domain/Redirect]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Transport/Wire]], [[src/PuduLangHttpClient/Utils/Template]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Hop]].

## Algorithm

1. The head is read until its blank line, within the head limit, then parsed for version, status, and headers.
2. A length body is read to its length, a chunked body through [[src/PuduLangHttpClient/Domain/Chunked]], and a close-framed body to the end.
3. A receiver gets each piece as it arrives, except for redirects being followed.
4. The connection is reusable only when the body ended cleanly and [[src/PuduLangHttpClient/Domain/Persistence]] agrees.

## Negative Logic (Prohibited Paths)

- A body over the buffer limit is refused, never truncated.

## Edge Cases

- A receiver answering `false` stops reading and the connection is closed.
- Whether any response byte arrived is reported, so a stale pooled connection can be told apart.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why read the framing incrementally instead of using `Std.Http.Client`?
  **A:** The standard client closes every connection after one exchange; reuse needs to know exactly where each response ends. _Rejected:_ `Std.Http.Client.send` (one connection per request).

## Referenced by

[[decisions/ADR-0002-owned-transport]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Domain/Chunked]] · [[src/PuduLangHttpClient/Domain/Persistence]] · [[src/PuduLangHttpClient/Domain/Redirect]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Transport/_MOC]] · [[src/PuduLangHttpClient/Utils/Template]]
