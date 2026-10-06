---
type: module
path: "@root/src/PuduLangHttpClient/Factory.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.9
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangHttpClient.Factory]
---

# PuduLangHttpClient.Factory

## Purpose

Named clients over handler pipelines the factory owns and renews: building and validating configuration, creating clients and handlers, typed clients, rotation, and closing expired pipelines.

## Interface

### Signatures

```pudu
export type Options = { listeners: Events.Listeners }

export type Invalid = { problems: Array[Str] }

export type Factory = { builders: Map[Str, Builder.Builder], defaults: Builder.Builder, state: Shared.Shared[State], bus: Bus.Bus }

export fn defaults() -> Options

export fn build(builders: Array[Builder.Builder]) -> Result[Factory, Invalid]

export fn buildWith(options: &Options, builders: Array[Builder.Builder]) -> Result[Factory, Invalid]

export fn explain(invalid: &Invalid) -> Str

export fn createClient(factory: &Factory, name: Str) -> Client.Client

export fn client(factory: &Factory) -> Client.Client

export fn typed[T](factory: &Factory, name: Str, make: fn(Client.Client) -> T) -> T

export fn createHandler(factory: &Factory, name: Str) -> Handler.Send

export fn names(factory: &Factory) -> Array[Str]

export fn statistics(factory: &Factory, name: Str) -> Option[Pool.Statistics]

export fn pendingExpired(factory: &Factory) -> Int

export fn dispose(factory: &Factory) -> ()

export fn rotatingHandler(factory: &Factory, name: Str) -> Handler.Send
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Constants/Messages]], [[src/PuduLangHttpClient/Domain/Expiry]], [[src/PuduLangHttpClient/Events]], [[src/PuduLangHttpClient/Events/Bus]], [[src/PuduLangHttpClient/Factory/Builder]], [[src/PuduLangHttpClient/Factory/Entry]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Pool]], [[src/PuduLangHttpClient/Utils/Shared]], [[src/PuduLangHttpClient/Utils/Template]], the standard library.
- **Consumed by:** the program.

## Algorithm

1. `buildWith` combines builders per name, applies the defaults first, validates every name, and reports every problem at once.
2. `createHandler` reuses a name's live pipeline until its lifetime runs out, then builds the next and retires the old one.
3. A retired pipeline's transport is closed once it has outlived one more lifetime.
4. Lifecycle events are published through [[src/PuduLangHttpClient/Events/Bus]].

## Negative Logic (Prohibited Paths)

- A name never configured gets the defaults rather than failing.

## Edge Cases

- Two threads rotating one name at once keep the first pipeline installed and close the other.

## Depth

DEPTH 0.9 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why close retired pipelines after a grace period instead of when no client uses them?
  **A:** Pudu has no weak references or finalizers to learn that the last client is gone; one more lifetime covers clients made just before rotation, and later sends still work by opening connections they close. _Rejected:_ never closing retired pipelines (leaks connections).
- **Q:** Why typed clients as a function from a client?
  **A:** A typed client is any record wrapping a client; a constructor function needs no registration and no reflection. _Rejected:_ registering typed clients by type.

## Referenced by

[[architecture/_MOC]] · [[decisions/ADR-0003-factory-owned-pipelines]] · [[domain/Lifetime]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Domain/Expiry]] · [[src/PuduLangHttpClient/Events]] · [[src/PuduLangHttpClient/Events/Bus]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Factory/Entry]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Pool]] · [[src/PuduLangHttpClient/Utils/Shared]] · [[src/PuduLangHttpClient/Utils/Template]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Factory]]
