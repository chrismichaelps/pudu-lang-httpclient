---
type: domain
tags: [domain]
---

# Lifetime

Each name has one live pipeline, numbered by generation. The [[src/PuduLangHttpClient/Factory]]
replaces it once its handler lifetime runs out and closes the retired one's transport after one more
lifetime. See [[decisions/ADR-0003-factory-owned-pipelines]].

## Referenced by

[[architecture/LANGUAGE]] · [[domain/_MOC]]
