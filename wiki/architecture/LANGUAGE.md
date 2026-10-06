---
type: language
tags: [architecture]
aliases: [Vocabulary]
---

# Architecture vocabulary

- **Module** — one `.pudu` file and its mirrored page under `wiki/src/`.
- **Request / Response / Content** — what is sent, what comes back, and a body with its headers. See [[domain/Message]].
- **Handler** — one delegating layer that sees a request, may call the rest, and sees the outcome. See [[seams/Handler]].
- **Primary handler** — the innermost sender: a transport, a stub, or the program's own. See [[seams/Primary]].
- **Pipeline** — handlers composed around a primary handler. See [[domain/Pipeline]].
- **Observer** — a pair of handlers placed outside and inside every other handler. See [[seams/Observer]].
- **Transport** — the pooled keep-alive primary handler. See [[domain/Connection]].
- **Route / Lease / Hop** — where a connection goes, permission to use one, and one request on it.
- **Builder / Entry / Generation** — a name's configuration, one built pipeline, and its number. See [[domain/Lifetime]].
- **Client** — a pipeline plus the defaults every request through it gets.
- **Listener / Event** — a plain function told about requests and pipelines. See [[domain/Events]] and [[seams/Listeners]].
- **Outcome / Failure** — a value or why there is none. See [[domain/Outcome]].

## Referenced by

[[architecture/_MOC]]
