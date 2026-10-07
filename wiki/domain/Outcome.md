---
type: domain
tags: [domain]
---

# Outcome

Every handler answers `Result[T, Failure]`. A failure names one cause: an invalid request, name
resolution, a connection or secured connection, a proxy tunnel, the protocol, a response cut short,
a body or head over its limit, too many redirects, a refused destination, an unsupported version, a
timeout, a cancellation, an unsuccessful status, an unreadable body, a resilience rejection, or a
crash. See [[src/PuduLangHttpClient]] and [[decisions/ADR-0001-outcomes-not-exceptions]].

## Referenced by

[[architecture/LANGUAGE]] · [[domain/_MOC]]
