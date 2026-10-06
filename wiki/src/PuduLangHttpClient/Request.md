---
type: module
path: "@root/src/PuduLangHttpClient/Request.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Request]
---

# PuduLangHttpClient.Request

## Purpose

What a client sends: method, address, version and policy, headers, optional body, options, and an optional receiver for a streamed response.

## Interface

### Signatures

```pudu
export type Receiver = fn(Bytes) -> Bool

export type Request = {
  method: Http.Method,
  uri: Str,
  version: Http.Version,
  versionPolicy: Version.Policy,
  headers: Array[(Str, Str)],
  content: Option[Content.Content],
  options: Options.Options,
  receiver: Option[Receiver]
}

export fn create(method: Http.Method, uri: Str) -> Request

export fn get(uri: Str) -> Request

export fn head(uri: Str) -> Request

export fn delete(uri: Str) -> Request

export fn post(uri: Str, content: Content.Content) -> Request

export fn put(uri: Str, content: Content.Content) -> Request

export fn patch(uri: Str, content: Content.Content) -> Request

export fn header(request: &Request, name: Str) -> Option[Str]

export fn option[T](request: &Request, name: &Options.Key[T]) -> Option[T]

export trait Shaping {
  fn withHeader(self: &Self, name: Str, value: Str) -> Self
  fn setHeader(self: &Self, name: Str, value: Str) -> Self
  fn removeHeader(self: &Self, name: Str) -> Self
  fn withContent(self: &Self, content: Content.Content) -> Self
  fn withVersion(self: &Self, version: Http.Version, policy: Version.Policy) -> Self
  fn withOption[T](self: &Self, name: &Options.Key[T], value: T) -> Self
  fn withReceiver(self: &Self, receiver: Receiver) -> Self
  fn withBearer(self: &Self, token: Str) -> Self
  fn accepting(self: &Self, mediaType: Str) -> Self
}
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Domain/Version]], [[src/PuduLangHttpClient/Options]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Client/Json]], [[src/PuduLangHttpClient/Endpoint]], [[src/PuduLangHttpClient/Events/Bus]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Handlers/Authorization]], [[src/PuduLangHttpClient/Handlers/Logging]], [[src/PuduLangHttpClient/Handlers/Metrics]], [[src/PuduLangHttpClient/Handlers/Propagation]], [[src/PuduLangHttpClient/Handlers/Resilience]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Stub]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Exchange]], [[src/PuduLangHttpClient/Transport/Hop]], [[src/PuduLangHttpClient/Transport/Writer]].

## Algorithm

1. Verb constructors build requests; `Shaping` changes one decision at a time.
2. `header` looks among the request's headers, then its body's.

## Negative Logic (Prohibited Paths)

- Building a request performs no effect.

## Edge Cases

- A receiver makes the transport hand the body over as it arrives instead of buffering it.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a trait chain for building?
  **A:** A request changes in several independent ways; methods read in the order decisions are made instead of nesting builder calls. _Rejected:_ nested combinator calls.

## Referenced by

[[domain/Message]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Client/Json]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Domain/Version]] · [[src/PuduLangHttpClient/Endpoint]] · [[src/PuduLangHttpClient/Events/Bus]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/Authorization]] · [[src/PuduLangHttpClient/Handlers/Logging]] · [[src/PuduLangHttpClient/Handlers/Metrics]] · [[src/PuduLangHttpClient/Handlers/Propagation]] · [[src/PuduLangHttpClient/Handlers/Resilience]] · [[src/PuduLangHttpClient/Options]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Stub]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Writer]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Messages]]
