---
type: module
path: "@root/src/PuduLangHttpClient/Constants/Events.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.3
depth_status: SHALLOW
tags: [module, leaf]
aliases: [PuduLangHttpClient.Constants.Events]
---

# PuduLangHttpClient.Constants.Events

## Purpose

The templates and source names of every structured log event.

## Interface

### Signatures

```pudu
export const PIPELINE_STARTED: Str

export const PIPELINE_ENDED: Str

export const REQUEST_SENDING: Str

export const RESPONSE_RECEIVED: Str

export const REQUEST_FAILED: Str

export const REQUEST_HEADERS: Str

export const RESPONSE_HEADERS: Str

export const LOGICAL_HANDLER: Str

export const CLIENT_HANDLER: Str

export const SOURCE_ROOT: Str
```

### Linkage

- **Requires:** nothing.
- **Consumed by:** [[src/PuduLangHttpClient/Handlers/Logging]].

## Algorithm

1. Templates name their properties in braces, escaped in Pudu text as `\{Name\}`.
2. Sources are `PuduLangHttpClient.<client>.LogicalHandler` and `PuduLangHttpClient.<client>.ClientHandler`.

## Negative Logic (Prohibited Paths)

- No log template is written outside this module.

## Edge Cases

- The unnamed client's sources omit the client segment.

## Depth

DEPTH 0.3 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why two sources per client?
  **A:** Events outside every handler show what the caller saw, events next to the primary handler show what went on the wire; filtering by source separates them. _Rejected:_ one source per client (retries would be indistinguishable from requests).

## Referenced by

[[src/PuduLangHttpClient/Constants/_MOC]] · [[src/PuduLangHttpClient/Handlers/Logging]]
