---
type: seam
capacity: LEAF
tags: [seam]
---

# Listeners (seam)

## Classification

Plain functions of an event, gathered in `Events.Listeners` ([[src/PuduLangHttpClient/Events]]).

## Adapters

- **Program** — any function, added with `onRequestStarted`, `onRequestCompleted`, `onRequestFailed`, and `onPipelineChanged`.

## Health

A listener never sees the mediator carrying events ([[decisions/ADR-0006-internal-event-bus]]).

## Referenced by

[[architecture/LANGUAGE]] · [[seams/_MOC]]
