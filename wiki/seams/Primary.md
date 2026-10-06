---
type: seam
capacity: CRITICAL
tags: [seam]
---

# Primary handler (seam)

## Classification

`Handler.Send` at the innermost position of a pipeline.

## Adapters

- **Transport** — [[src/PuduLangHttpClient/Transport]], the default.
- **Stub** — [[src/PuduLangHttpClient/Stub]], for tests.
- **Program** — any sender, with `withPrimary`.

## Health

Everything above the primary handler is tested without sockets by swapping in the stub.

## Referenced by

[[architecture/LANGUAGE]] · [[seams/_MOC]]
