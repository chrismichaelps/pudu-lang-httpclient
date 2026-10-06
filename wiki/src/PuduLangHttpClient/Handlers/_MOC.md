---
type: moc
tags: [moc]
---

# PuduLangHttpClient.Handlers

- [[src/PuduLangHttpClient/Handlers/Authorization]] — Credentials added to requests that carry none: basic credentials, or bearer tokens fetched again when the server refuses them.
- [[src/PuduLangHttpClient/Handlers/Logging]] — Structured events around a pipeline and around its primary handler, written to a pudu-lang-log logger.
- [[src/PuduLangHttpClient/Handlers/Metrics]] — Counts and durations of the requests through a pipeline, kept in a shared snapshot.
- [[src/PuduLangHttpClient/Handlers/Propagation]] — Headers of the operation in progress copied onto outgoing requests, under the same or another name.
- [[src/PuduLangHttpClient/Handlers/Resilience]] — Requests run through pudu-lang-resilience pipelines tuned for HTTP: one pipeline, one per request, one from a registry, the standard pipeline, and the standard hedging pipeline.

## Referenced by

[[src/PuduLangHttpClient/_MOC]]
