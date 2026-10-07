---
type: moc
tags: [moc]
---

# PuduLangHttpClient.Transport

- [[src/PuduLangHttpClient/Transport/Connect]] — Routes chosen per endpoint and connections opened directly or through a proxy, within a connect timeout.
- [[src/PuduLangHttpClient/Transport/Exchange]] — One request written and one response read over a connection: interim responses skipped, the head parsed, and the body read by its framing, buffered or streamed.
- [[src/PuduLangHttpClient/Transport/Hop]] — One request sent over a pooled connection, retried once on a new connection when a reused one turns out to have been closed while idle.
- [[src/PuduLangHttpClient/Transport/Pool]] — Idle connections kept per origin and handed out again, within a per-origin limit, swept by lifetime and idle time.
- [[src/PuduLangHttpClient/Transport/Wire]] — One open connection, plain or secured, with every operation bounded by the request's deadline.
- [[src/PuduLangHttpClient/Transport/Writer]] — A request written as the bytes that go on the wire.

## Referenced by

[[src/PuduLangHttpClient/_MOC]]
