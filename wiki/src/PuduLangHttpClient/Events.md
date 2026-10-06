---
type: module
path: "@root/src/PuduLangHttpClient/Events.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Events]
---

# PuduLangHttpClient.Events

## Purpose

What happens to requests and pipelines, and the listeners told about it.

## Interface

### Signatures

```pudu
export type RequestStarted = { client: Str, method: Str, uri: Str } derives Json.Encode

export type RequestCompleted = { client: Str, method: Str, uri: Str, status: Int, elapsed: Int } derives Json.Encode

export type RequestFailed = { client: Str, method: Str, uri: Str, failure: Str, elapsed: Int } derives Json.Encode

export type Stage = Created | Expired | Closed derives Json.Encode

export type PipelineChanged = { client: Str, generation: Int, stage: Stage } derives Json.Encode

export type Listeners = {
  started: Array[fn(RequestStarted) -> ()],
  completed: Array[fn(RequestCompleted) -> ()],
  failed: Array[fn(RequestFailed) -> ()],
  changed: Array[fn(PipelineChanged) -> ()]
}

export fn listeners() -> Listeners

export fn hearsRequests(given: &Listeners) -> Bool

export trait Listening {
  fn onRequestStarted(self: &Self, listen: fn(RequestStarted) -> ()) -> Self
  fn onRequestCompleted(self: &Self, listen: fn(RequestCompleted) -> ()) -> Self
  fn onRequestFailed(self: &Self, listen: fn(RequestFailed) -> ()) -> Self
  fn onPipelineChanged(self: &Self, listen: fn(PipelineChanged) -> ()) -> Self
}
```

### Linkage

- **Requires:** the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Events/Bus]], [[src/PuduLangHttpClient/Factory]].

## Algorithm

1. Events are records deriving `Json.Encode`.
2. `Listening` adds plain functions per kind of event.

## Negative Logic (Prohibited Paths)

- Listeners never see or need the mediator that carries the events.

## Edge Cases

- A listener of pipeline changes alone adds no handler to request pipelines.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why plain functions instead of mediator registrations?
  **A:** A program using the client should not have to know how events are carried; the mediator is the package's own concern ([[decisions/ADR-0006-internal-event-bus]]). _Rejected:_ asking callers to build a mediator.

## Referenced by

[[decisions/ADR-0006-internal-event-bus]] · [[domain/Events]] · [[seams/Listeners]] · [[src/PuduLangHttpClient/Events/Bus]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Factory]]
