---
type: module
path: "@root/src/PuduLangHttpClient/Constants/Defaults.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.3
depth_status: SHALLOW
tags: [module, leaf]
aliases: [PuduLangHttpClient.Constants.Defaults]
---

# PuduLangHttpClient.Constants.Defaults

## Purpose

The numbers every option starts from: timeouts, limits, lifetimes, ports, and the sentinels for no limit.

## Interface

### Signatures

```pudu
export const INFINITE: Int

export const UNLIMITED: Int

export const DEFAULT_NAME: Str

export const CLIENT_TIMEOUT: Int

export const MAX_RESPONSE_BUFFER: Int

export const HANDLER_LIFETIME: Int

export const POOLED_IDLE_TIMEOUT: Int

export const POOLED_LIFETIME: Int

export const MAX_CONNECTIONS_PER_SERVER: Int

export const CONNECT_TIMEOUT: Int

export const MAX_REDIRECTS: Int

export const MAX_RESPONSE_HEADERS: Int

export const READ_CHUNK: Int

export const POOL_WAIT_SLICE: Int

export const ABORT_POLL: Int

export const HTTP_PORT: Int

export const HTTPS_PORT: Int
```

### Linkage

- **Requires:** nothing.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Domain/Expiry]], [[src/PuduLangHttpClient/Domain/Uri]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Factory/Builder]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Abort]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Exchange]], [[src/PuduLangHttpClient/Transport/Pool]], [[src/PuduLangHttpClient/Transport/Wire]], [[src/PuduLangHttpClient/Utils/Tokens]].

## Algorithm

1. `INFINITE` (-1) is a duration that never elapses; `UNLIMITED` is the largest count.

## Negative Logic (Prohibited Paths)

- No default is written as a literal in another module.

## Edge Cases

- `CONNECT_TIMEOUT` is infinite: a connection is still bounded by the request's own deadline.

## Depth

DEPTH 0.3 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a 64 MiB response buffer rather than an unbounded one?
  **A:** A client that buffers whatever a server sends lets that server exhaust the caller's memory; 64 MiB is generous for buffered bodies, and larger ones belong in a receiver. _Rejected:_ the largest `Int` (one response could take the process down).
- **Q:** Why a two-minute handler lifetime?
  **A:** It bounds how long a pipeline keeps resolving a host to stale addresses while still reusing connections for many requests. _Rejected:_ an infinite lifetime by default (DNS changes would never be seen).

## Referenced by

[[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/_MOC]] · [[src/PuduLangHttpClient/Domain/Expiry]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Abort]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Pool]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Utils/Tokens]]
