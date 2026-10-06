---
type: module
path: "@root/src/PuduLangHttpClient/Factory/Builder.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangHttpClient.Factory.Builder]
---

# PuduLangHttpClient.Factory.Builder

## Purpose

How one named client, or every client, is configured: client actions, handlers, the primary handler, transport edits, the handler lifetime, observers, and redaction.

## Interface

### Signatures

```pudu
export type Primary = Pooled(Transport.Options) | Custom(fn() -> Handler.Send)

export type Observation = { name: Str, redact: fn(Str) -> Bool }

export type Observer = { outer: fn(Observation) -> Handler.Handler, inner: fn(Observation) -> Handler.Handler }

export type Builder = {
  name: Str,
  everyClient: Bool,
  clientActions: Array[fn(Client.Client) -> Client.Client],
  handlers: Array[fn() -> Handler.Handler],
  handlerEdits: Array[fn(Array[Handler.Handler]) -> Array[Handler.Handler]],
  primary: Option[Primary],
  transportEdits: Array[fn(Transport.Options) -> Transport.Options],
  lifetime: Option[Int],
  observers: Array[Observer],
  observersCleared: Bool,
  redacted: Array[Str],
  redactWhen: Option[fn(Str) -> Bool]
}

export fn named(name: Str) -> Builder

export fn defaults() -> Builder

export fn combine(first: &Builder, second: &Builder) -> Builder

export fn lifetimeOf(builder: &Builder) -> Int

export fn redaction(builder: &Builder) -> fn(Str) -> Bool

export trait Configuring {
  fn withBaseAddress(self: &Self, address: Str) -> Self
  fn withDefaultHeader(self: &Self, name: Str, value: Str) -> Self
  fn withTimeout(self: &Self, millis: Int) -> Self
  fn withMaxResponseContentBufferSize(self: &Self, bytes: Int) -> Self
  fn withDefaultVersion(self: &Self, version: Http.Version, policy: Version.Policy) -> Self
  fn configureClient(self: &Self, action: fn(Client.Client) -> Client.Client) -> Self
  fn withHandler(self: &Self, handler: Handler.Handler) -> Self
  fn withHandlerFactory(self: &Self, make: fn() -> Handler.Handler) -> Self
  fn configureHandlers(self: &Self, edit: fn(Array[Handler.Handler]) -> Array[Handler.Handler]) -> Self
  fn withTransport(self: &Self, options: Transport.Options) -> Self
  fn configureTransport(self: &Self, edit: fn(Transport.Options) -> Transport.Options) -> Self
  fn withPrimary(self: &Self, make: fn() -> Handler.Send) -> Self
  fn withHandlerLifetime(self: &Self, millis: Int) -> Self
  fn observedBy(self: &Self, observer: Observer) -> Self
  fn withoutObservers(self: &Self) -> Self
  fn redactingHeaders(self: &Self, names: &Array[Str]) -> Self
  fn redactingWhen(self: &Self, test: fn(Str) -> Bool) -> Self
}
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Domain/Version]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Transport]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Events/Bus]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Factory/Entry]], [[src/PuduLangHttpClient/Handlers/Logging]].

## Algorithm

1. Settings that add (actions, handlers, edits, observers, redacted names) are joined in order; settings that replace (primary, lifetime, predicate) are taken from the later builder.
2. `withoutObservers` clears every observer added before, including the defaults'.
3. `redaction` hides sensitive headers always, plus named ones and those a predicate holds for.

## Negative Logic (Prohibited Paths)

- A builder performs no effect; pipelines are built by the factory.

## Edge Cases

- The defaults' builder applies first to every name.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why redact `authorization`, `cookie`, and `set-cookie` always?
  **A:** Logged credentials outlive the request and reach people who should never see them. _Rejected:_ redacting only what the program names.

## Referenced by

[[architecture/_MOC]] · [[decisions/ADR-0007-secure-defaults]] · [[seams/Observer]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Domain/Version]] · [[src/PuduLangHttpClient/Events/Bus]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Factory/Entry]] · [[src/PuduLangHttpClient/Factory/_MOC]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/Logging]] · [[src/PuduLangHttpClient/Transport]] · [[subsystems/Factory]]
