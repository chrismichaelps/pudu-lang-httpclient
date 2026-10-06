---
type: module
path: "@root/src/PuduLangHttpClient/Handler.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, backbone]
aliases: [PuduLangHttpClient.Handler]
---

# PuduLangHttpClient.Handler

## Purpose

Delegating handlers and their composition around a primary handler.

## Interface

### Signatures

```pudu
export type Send = fn(Request.Request, Cancel.Token) -> HttpClient.Outcome[Response.Response]

export type Handler = fn(Request.Request, Cancel.Token, Send) -> HttpClient.Outcome[Response.Response]

export fn compose(handlers: &Array[Handler], primary: Send) -> Send

export fn around(handler: Handler, inner: Send) -> Send

export fn before(step: fn(Request.Request) -> Request.Request) -> Handler

export fn after(step: fn(Response.Response) -> Response.Response) -> Handler

export fn passThrough() -> Handler
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Events/Bus]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Factory/Builder]], [[src/PuduLangHttpClient/Factory/Entry]], [[src/PuduLangHttpClient/Handlers/Authorization]], [[src/PuduLangHttpClient/Handlers/Logging]], [[src/PuduLangHttpClient/Handlers/Metrics]], [[src/PuduLangHttpClient/Handlers/Propagation]], [[src/PuduLangHttpClient/Handlers/Resilience]], [[src/PuduLangHttpClient/Stub]], [[src/PuduLangHttpClient/Transport]].

## Algorithm

1. `Send` is the primary handler's shape; `Handler` sees the request, may call the rest, and sees the outcome.
2. `compose` wraps handlers from the innermost out, so the first is outermost.

## Negative Logic (Prohibited Paths)

- A handler never reaches the transport except through the `Send` it is given.

## Edge Cases

- No handlers leave the primary handler alone.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why functions rather than a handler trait?
  **A:** A handler has one operation; a function value composes, stores in a builder, and closes over its own state without a type per handler. _Rejected:_ a trait with a `send` member.

## Referenced by

[[architecture/_MOC]] · [[domain/Pipeline]] · [[seams/Handler]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Events/Bus]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Factory/Entry]] · [[src/PuduLangHttpClient/Handlers/Authorization]] · [[src/PuduLangHttpClient/Handlers/Logging]] · [[src/PuduLangHttpClient/Handlers/Metrics]] · [[src/PuduLangHttpClient/Handlers/Propagation]] · [[src/PuduLangHttpClient/Handlers/Resilience]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Stub]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Messages]]
