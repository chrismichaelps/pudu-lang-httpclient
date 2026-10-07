---
type: domain
tags: [domain]
---

# Message

A [[src/PuduLangHttpClient/Request]] is a method, an address, a version and policy, headers, an
optional [[src/PuduLangHttpClient/Content]] body, typed [[src/PuduLangHttpClient/Options]], and an
optional receiver for a streamed response. A [[src/PuduLangHttpClient/Response]] carries the status,
headers, body, trailers, and the final request after redirects. A body is bytes plus content headers,
or a producer streamed in chunks.

## Referenced by

[[architecture/LANGUAGE]] · [[domain/_MOC]]
