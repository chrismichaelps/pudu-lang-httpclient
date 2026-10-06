---
type: module
path: "@root/src/PuduLangHttpClient/Handlers/Metrics.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.4
depth_status: SHALLOW
tags: [module, leaf]
aliases: [PuduLangHttpClient.Handlers.Metrics]
---

# PuduLangHttpClient.Handlers.Metrics

## Purpose

Counts and durations of the requests through a pipeline, kept in a shared snapshot.

## Interface

### Signatures

```pudu
export type Snapshot = {
  requests: Int,
  active: Int,
  informational: Int,
  successful: Int,
  redirected: Int,
  clientErrors: Int,
  serverErrors: Int,
  failed: Int,
  totalMillis: Int,
  longestMillis: Int
} derives Json.Encode

export type Meter = { held: Shared.Shared[Snapshot] }

export fn meter() -> Meter

export fn handler(target: &Meter) -> Handler.Handler

export fn snapshot(source: &Meter) -> Snapshot
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Utils/Shared]], the standard library.
- **Consumed by:** the program.

## Algorithm

1. Each request counts as started and in flight, then by status class or as failed, with its duration added and the longest kept.

## Negative Logic (Prohibited Paths)

- Measuring never changes an outcome.

## Edge Cases

- The snapshot derives `Json.Encode` so it can be exported as it is.

## Depth

DEPTH 0.4 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a snapshot rather than a metrics library?
  **A:** The counts a dashboard needs fit in one record; exporting it is the program's choice. _Rejected:_ a dependency on a metrics package.

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/_MOC]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Utils/Shared]] · [[subsystems/Handlers]]
