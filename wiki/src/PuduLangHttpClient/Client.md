---
type: module
path: "@root/src/PuduLangHttpClient/Client.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangHttpClient.Client]
---

# PuduLangHttpClient.Client

## Purpose

A configured client: a pipeline plus the base address, default headers, timeout, buffer limit, and default version every request gets, with helpers for each verb.

## Interface

### Signatures

```pudu
export type Client = {
  name: Str,
  send: Handler.Send,
  baseAddress: Option[Str],
  defaultHeaders: Array[(Str, Str)],
  timeout: Int,
  maxResponseContentBufferSize: Int,
  defaultVersion: Http.Version,
  defaultVersionPolicy: Version.Policy,
  pending: Shared.Shared[Cancel.Token]
}

export fn create(sender: Handler.Send) -> Client

export fn over(transport: &Transport.Transport) -> Client

export fn pooled(options: Transport.Options) -> Client

export fn validate(client: &Client) -> Array[Str]

export trait Configuring {
  fn withBaseAddress(self: &Self, address: Str) -> Self
  fn withDefaultHeader(self: &Self, name: Str, value: Str) -> Self
  fn withTimeout(self: &Self, millis: Int) -> Self
  fn withMaxResponseContentBufferSize(self: &Self, bytes: Int) -> Self
  fn withDefaultVersion(self: &Self, version: Http.Version, policy: Version.Policy) -> Self
  fn named(self: &Self, name: Str) -> Self
}

export fn send(client: &Client, request: Request.Request) -> HttpClient.Outcome[Response.Response]

export fn sendWith(client: &Client, request: Request.Request, token: Cancel.Token) -> HttpClient.Outcome[Response.Response]

export fn cancelPending(client: &Client) -> ()

export fn build(client: &Client, method: Http.Method, uri: Str) -> Request.Request

export fn get(client: &Client, uri: Str) -> HttpClient.Outcome[Response.Response]

export fn getText(client: &Client, uri: Str) -> HttpClient.Outcome[Str]

export fn getBytes(client: &Client, uri: Str) -> HttpClient.Outcome[Bytes]

export fn stream(client: &Client, uri: Str, receiver: Request.Receiver) -> HttpClient.Outcome[Response.Response]

export fn head(client: &Client, uri: Str) -> HttpClient.Outcome[Response.Response]

export fn post(client: &Client, uri: Str, content: Content.Content) -> HttpClient.Outcome[Response.Response]

export fn put(client: &Client, uri: Str, content: Content.Content) -> HttpClient.Outcome[Response.Response]

export fn patch(client: &Client, uri: Str, content: Content.Content) -> HttpClient.Outcome[Response.Response]

export fn delete(client: &Client, uri: Str) -> HttpClient.Outcome[Response.Response]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Constants/Messages]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Domain/Uri]], [[src/PuduLangHttpClient/Domain/Version]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Options]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Utils/Shared]], [[src/PuduLangHttpClient/Utils/Template]], [[src/PuduLangHttpClient/Utils/Tokens]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client/Json]], [[src/PuduLangHttpClient/Endpoint]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Factory/Builder]].

## Algorithm

1. `sendWith` resolves the address, merges default headers, sets the buffer limit, and links the caller's token, the pending token, and the timeout.
2. A timeout of the client's own is reported as `TimedOut(timeout)`.
3. `cancelPending` cancels every request in flight and swaps in a fresh token.

## Negative Logic (Prohibited Paths)

- A relative address without a base address is refused before anything is sent.

## Edge Cases

- Helpers send with the client's default version; requests built by the caller keep their own.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why are clients cheap values?
  **A:** A client is a pipeline reference and settings; making one per use lets the factory rotate pipelines underneath without the caller noticing. _Rejected:_ long-lived mutable clients.
- **Q:** How is one client configured once and shared?
  **A:** `pooled` (or `over` a transport) builds it at startup; every copy sends through the same pool, so passing the value around shares connections. A pooled connection lifetime keeps a long-lived client seeing address changes. _Rejected:_ a global client in module scope (module scope holds only constants).

## Referenced by

[[architecture/_MOC]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client/Json]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Domain/Version]] · [[src/PuduLangHttpClient/Endpoint]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Options]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Utils/Shared]] · [[src/PuduLangHttpClient/Utils/Template]] · [[src/PuduLangHttpClient/Utils/Tokens]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Factory]]
