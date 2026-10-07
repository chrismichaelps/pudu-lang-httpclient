---
type: domain
tags: [domain]
---

# Connection

The [[src/PuduLangHttpClient/Transport]] keeps idle connections per route
([[src/PuduLangHttpClient/Transport/Pool]]) and reuses the most recently released one. A connection
is kept only when the exchange ended cleanly and neither side asked to close it
([[src/PuduLangHttpClient/Domain/Persistence]]); it is closed once it outlives its pooled lifetime or
idle timeout, and an origin never holds more than its per-server limit. See
[[decisions/ADR-0002-owned-transport]].

## Referenced by

[[architecture/LANGUAGE]] · [[domain/_MOC]]
