---
type: module
path: "@root/src/PuduLangHttpClient/Events/Bus.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Events.Bus]
---

# PuduLangHttpClient.Events.Bus

## Purpose

The mediator the package owns: listeners registered as notification handlers, and events published through it.

## Interface

### Signatures

```pudu
export type Bus = { mediator: Mediator.Mediator, kinds: Kinds, hearsRequests: Bool }

export fn create(given: &Events.Listeners) -> Bus

export fn pipelineChanged(bus: &Bus, client: Str, generation: Int, stage: Events.Stage) -> ()

export fn observer(bus: &Bus) -> Option[Builder.Observer]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Events]], [[src/PuduLangHttpClient/Factory/Builder]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], `pudu-lang-mediator`, the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Factory]].

## Algorithm

1. `create` makes one notification kind per event and registers each listener under its own name.
2. `observer` publishes a request's start and its completion or failure, and is present only when someone listens to requests.

## Negative Logic (Prohibited Paths)

- Publishing never changes a request's outcome.

## Edge Cases

- A bus without listeners still publishes pipeline changes, reaching nobody.

## Depth

DEPTH 0.6 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why carry events through a mediator at all?
  **A:** It gives ordered delivery to any number of listeners and a place to add behaviors to event delivery later, without the factory knowing who listens. _Rejected:_ calling each listener from the factory directly.

## Referenced by

[[decisions/ADR-0006-internal-event-bus]] · [[seams/Observer]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Events]] · [[src/PuduLangHttpClient/Events/_MOC]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]]
