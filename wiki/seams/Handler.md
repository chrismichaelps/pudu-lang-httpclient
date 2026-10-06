---
type: seam
capacity: BACKBONE
tags: [seam]
---

# Handler (seam)

## Classification

`Handler.Handler` — a function of the request, the token, and the rest of the pipeline
([[src/PuduLangHttpClient/Handler]]).

## Adapters

- **Package handlers** — [[subsystems/Handlers]].
- **Program handlers** — any function of the shape, added with `withHandler` or `withHandlerFactory`.

## Health

A handler reaches the wire only through the sender it is given.

## Referenced by

[[architecture/LANGUAGE]] · [[domain/Pipeline]] · [[seams/_MOC]]
