---
type: module
path: "@root/src/PuduLangHttpClient/Handlers/Authorization.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.4
depth_status: SHALLOW
tags: [module, leaf]
aliases: [PuduLangHttpClient.Handlers.Authorization]
---

# PuduLangHttpClient.Handlers.Authorization

## Purpose

Credentials added to requests that carry none: basic credentials, or bearer tokens fetched again when the server refuses them.

## Interface

### Signatures

```pudu
export type Tokens = fn(Bool) -> HttpClient.Outcome[Str]

export fn basic(user: Str, password: Str) -> Handler.Handler

export fn bearer(tokens: Tokens) -> Handler.Handler
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], the standard library.
- **Consumed by:** the program.

## Algorithm

1. `bearer` fetches a token, sends, and on 401 fetches a fresh token and sends once more.

## Negative Logic (Prohibited Paths)

- A request carrying its own `authorization` is left alone.
- A second 401 is answered as it is.

## Edge Cases

- A token that cannot be fetched fails the request with the fetcher's failure.

## Depth

DEPTH 0.4 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why retry once on 401?
  **A:** An expired token is the common cause; a second refusal means the credentials are wrong and repeating cannot help. _Rejected:_ retrying until success.

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/_MOC]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[subsystems/Handlers]]
