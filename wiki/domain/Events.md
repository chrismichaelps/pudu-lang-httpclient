---
type: domain
tags: [domain]
---

# Events

A request through a factory's pipeline is announced when it starts and when it completes or fails;
a pipeline is announced when it is created, expires, and closes ([[src/PuduLangHttpClient/Events]]).
Every event derives `Json.Encode`. See [[decisions/ADR-0006-internal-event-bus]].

## Referenced by

[[CHANGELOG]] · [[architecture/LANGUAGE]] · [[domain/_MOC]]
