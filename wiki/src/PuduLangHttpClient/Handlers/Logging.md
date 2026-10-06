---
type: module
path: "@root/src/PuduLangHttpClient/Handlers/Logging.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Handlers.Logging]
---

# PuduLangHttpClient.Handlers.Logging

## Purpose

Structured events around a pipeline and around its primary handler, written to a pudu-lang-log logger.

## Interface

### Signatures

```pudu
export fn observer(logger: Logger.Logger) -> Builder.Observer

export fn sourceOf(name: Str, layer: Str) -> Str
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Constants/Events]], [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Factory/Builder]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], `pudu-lang-log`, the standard library.
- **Consumed by:** the program.

## Algorithm

1. The outer handler writes start and end events under `LogicalHandler`; the inner one under `ClientHandler`.
2. Headers are listed at the verbose level with redacted values.

## Negative Logic (Prohibited Paths)

- Logging never changes an outcome.

## Edge Cases

- The unnamed client's sources omit the client segment.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why an observer rather than a plain handler?
  **A:** The two positions are what make the events useful; an observer places both from one call. _Rejected:_ two separate handlers the program must place.

## Referenced by

[[decisions/ADR-0005-integrations-at-the-edge]] · [[seams/Observer]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Constants/Events]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/_MOC]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[subsystems/Handlers]]
