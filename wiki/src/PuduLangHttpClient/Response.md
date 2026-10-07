---
type: module
path: "@root/src/PuduLangHttpClient/Response.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Response]
---

# PuduLangHttpClient.Response

## Purpose

What came back: status, version, headers, body, trailers, and the final request after redirects, with readers and a success check.

## Interface

### Signatures

```pudu
export type Response = {
  status: Http.Status,
  version: Http.Version,
  headers: Array[(Str, Str)],
  content: Content.Content,
  trailers: Array[(Str, Str)],
  request: Request.Request
}

export fn answer(request: &Request.Request, code: Int) -> Response

export fn isSuccess(response: &Response) -> Bool

export fn ensureSuccess(response: Response) -> HttpClient.Outcome[Response]

export fn header(response: &Response, name: Str) -> Option[Str]

export fn headerValues(response: &Response, name: Str) -> Array[Str]

export fn text(response: &Response) -> HttpClient.Outcome[Str]

export fn bytes(response: &Response) -> Bytes

export fn json(response: &Response) -> HttpClient.Outcome[Json.Json]

export fn readAs[T: Json.Decode](response: &Response) -> HttpClient.Outcome[T]

export fn retryAfter(response: &Response, now: Int) -> Option[Int]

export trait Answering {
  fn withHeader(self: &Self, name: Str, value: Str) -> Self
  fn withContent(self: &Self, content: Content.Content) -> Self
  fn withText(self: &Self, body: Str) -> Self
  fn withJson(self: &Self, value: &Json.Json) -> Self
}
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Domain/RetryAfter]], [[src/PuduLangHttpClient/Request]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Client/Json]], [[src/PuduLangHttpClient/Endpoint]], [[src/PuduLangHttpClient/Events/Bus]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Handlers/Authorization]], [[src/PuduLangHttpClient/Handlers/Logging]], [[src/PuduLangHttpClient/Handlers/Metrics]], [[src/PuduLangHttpClient/Handlers/Propagation]], [[src/PuduLangHttpClient/Handlers/Resilience]], [[src/PuduLangHttpClient/Stub]], [[src/PuduLangHttpClient/Transport]].

## Algorithm

1. `ensureSuccess` turns a status outside 2xx into `Unsuccessful`.
2. `Answering` builds responses for stubs; content headers go with the body.

## Negative Logic (Prohibited Paths)

- A non-success status is never a failure until `ensureSuccess` is asked.

## Edge Cases

- `retryAfter` is measured from the given wall-clock time.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why keep the final request in the response?
  **A:** After redirects the caller needs to know which address answered. _Rejected:_ returning only the original request.

## Referenced by

[[domain/Message]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Client/Json]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Domain/RetryAfter]] · [[src/PuduLangHttpClient/Endpoint]] · [[src/PuduLangHttpClient/Events/Bus]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/Authorization]] · [[src/PuduLangHttpClient/Handlers/Logging]] · [[src/PuduLangHttpClient/Handlers/Metrics]] · [[src/PuduLangHttpClient/Handlers/Propagation]] · [[src/PuduLangHttpClient/Handlers/Resilience]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Stub]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Messages]]
