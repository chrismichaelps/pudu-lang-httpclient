---
type: domain
tags: [domain]
---

# Pipeline

Handlers composed around a primary handler ([[src/PuduLangHttpClient/Handler]]). In a factory's
pipeline the order is fixed ([[src/PuduLangHttpClient/Factory/Entry]]): the event bus's observer,
then the program's observers' outer handlers, then the added handlers as rearranged, then the
observers' inner handlers in reverse, then the primary handler. See [[seams/Handler]] and
[[seams/Observer]].

## Referenced by

[[architecture/LANGUAGE]] · [[domain/_MOC]]
