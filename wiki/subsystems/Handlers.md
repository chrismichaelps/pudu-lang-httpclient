---
type: subsystem
tags: [subsystem]
---

# Handlers

[[src/PuduLangHttpClient/Handlers/Logging]], [[src/PuduLangHttpClient/Handlers/Resilience]],
[[src/PuduLangHttpClient/Handlers/Propagation]], [[src/PuduLangHttpClient/Handlers/Authorization]],
and [[src/PuduLangHttpClient/Handlers/Metrics]] — the only modules besides the event bus that
import other packages ([[decisions/ADR-0005-integrations-at-the-edge]]).

## Referenced by

[[CHANGELOG]] · [[decisions/ADR-0005-integrations-at-the-edge]] · [[seams/Handler]] · [[subsystems/_MOC]]
