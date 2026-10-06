---
type: moc
tags: [moc]
---

# PuduLangHttpClient

- [[src/PuduLangHttpClient/Client]] — A configured client: a pipeline plus the base address, default headers, timeout, buffer limit, and default version every request gets, with helpers for each verb.
- [[src/PuduLangHttpClient/Content]] — Message bodies and the headers that describe them: text, bytes, JSON documents and derived values, forms, and multipart bodies, plus readers for each.
- [[src/PuduLangHttpClient/Cookies]] — A jar of cookies shared by the requests of one transport, saved and restored as JSON.
- [[src/PuduLangHttpClient/Endpoint]] — Declarative endpoints: a method and an address template whose `{name}` slots are filled from arguments, sent and read as JSON.
- [[src/PuduLangHttpClient/Events]] — What happens to requests and pipelines, and the listeners told about it.
- [[src/PuduLangHttpClient/Factory]] — Named clients over handler pipelines the factory owns and renews: building and validating configuration, creating clients and handlers, typed clients, rotation, and closing expired pipelines.
- [[src/PuduLangHttpClient/Handler]] — Delegating handlers and their composition around a primary handler.
- [[src/PuduLangHttpClient/Options]] — Typed values a request carries through its handlers, keyed by name and written as text.
- [[src/PuduLangHttpClient/Proxy]] — A forward proxy: its address, credentials, and the hosts reached around it.
- [[src/PuduLangHttpClient/Request]] — What a client sends: method, address, version and policy, headers, optional body, options, and an optional receiver for a streamed response.
- [[src/PuduLangHttpClient/Response]] — What came back: status, version, headers, body, trailers, and the final request after redirects, with readers and a success check.
- [[src/PuduLangHttpClient/Stub]] — A primary handler answering from rules, recording every request, for tests.
- [[src/PuduLangHttpClient/Transport]] — The primary handler: pooled keep-alive connections, redirects, cookies, decompression, proxies, an address policy, and response limits.
- [[src/PuduLangHttpClient/Client/_MOC]] — the modules under `PuduLangHttpClient.Client`.
- [[src/PuduLangHttpClient/Constants/_MOC]] — the modules under `PuduLangHttpClient.Constants`.
- [[src/PuduLangHttpClient/Domain/_MOC]] — the modules under `PuduLangHttpClient.Domain`.
- [[src/PuduLangHttpClient/Events/_MOC]] — the modules under `PuduLangHttpClient.Events`.
- [[src/PuduLangHttpClient/Factory/_MOC]] — the modules under `PuduLangHttpClient.Factory`.
- [[src/PuduLangHttpClient/Handlers/_MOC]] — the modules under `PuduLangHttpClient.Handlers`.
- [[src/PuduLangHttpClient/Transport/_MOC]] — the modules under `PuduLangHttpClient.Transport`.
- [[src/PuduLangHttpClient/Utils/_MOC]] — the modules under `PuduLangHttpClient.Utils`.

## Referenced by

[[src/_MOC]]
