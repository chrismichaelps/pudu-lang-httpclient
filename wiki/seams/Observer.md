---
type: seam
capacity: LEAF
tags: [seam]
---

# Observer (seam)

## Classification

`Builder.Observer` — two handler factories, placed outside and inside every other handler
([[src/PuduLangHttpClient/Factory/Builder]]).

## Adapters

- **Logging** — [[src/PuduLangHttpClient/Handlers/Logging]].
- **Event bus** — [[src/PuduLangHttpClient/Events/Bus]].
- **Program** — any pair, with `observedBy`; `withoutObservers` removes them.

## Health

An observer sees both what the caller sent and what went on the wire.

## Referenced by

[[architecture/LANGUAGE]] · [[domain/Pipeline]] · [[seams/_MOC]]
