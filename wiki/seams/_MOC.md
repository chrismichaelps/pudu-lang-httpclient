---
type: moc
tags: [moc, seam]
---

# Seams

| Seam | Capacity | Adapters |
| --- | --- | --- |
| [[seams/Handler]] | BACKBONE | the package's handlers, any program function of the handler shape |
| [[seams/Primary]] | CRITICAL | the pooled transport, the stub, any program sender |
| [[seams/Observer]] | LEAF | logging, the event bus, any program observer |
| [[seams/Listeners]] | LEAF | any program function of an event |

## Referenced by

[[00-INDEX]]
