---
type: module
path: "@root/src/PuduLangHttpClient/Stub.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Stub]
---

# PuduLangHttpClient.Stub

## Purpose

A primary handler answering from rules, recording every request, for tests.

## Interface

### Signatures

```pudu
export type Rule = { matches: fn(Request.Request) -> Bool, answer: fn(Request.Request) -> HttpClient.Outcome[Response.Response] }

export type Stub = { rules: Shared.Shared[Array[Rule]], received: Shared.Shared[Array[Request.Request]] }

export fn create() -> Stub

export fn when(stub: &Stub, matches: fn(Request.Request) -> Bool, answer: fn(Request.Request) -> HttpClient.Outcome[Response.Response]) -> ()

export fn on(stub: &Stub, method: Http.Method, path: Str, answer: fn(Request.Request) -> HttpClient.Outcome[Response.Response]) -> ()

export fn respond(stub: &Stub, method: Http.Method, path: Str, code: Int, body: Str) -> ()

export fn send(stub: &Stub) -> Handler.Send

export fn received(stub: &Stub) -> Array[Request.Request]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Domain/Uri]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Utils/Shared]], the standard library.
- **Consumed by:** the program.

## Algorithm

1. Rules are tried in the order added; none matching answers 404.
2. A request's receiver gets the body it is answered and the response keeps none.

## Negative Logic (Prohibited Paths)

- A cancelled request is refused with the caller's reason and not recorded; a passed deadline is a timeout.

## Edge Cases

- A rule may answer a failure.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a stub primary handler rather than a fake server?
  **A:** Handlers and clients are tested without sockets, threads, or ports; the stub sits exactly where the transport would. _Rejected:_ a loopback server for every test.

## Referenced by

[[seams/Primary]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Utils/Shared]] · [[src/PuduLangHttpClient/_MOC]]
