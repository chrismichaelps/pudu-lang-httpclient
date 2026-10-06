---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Redirect.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Redirect]
---

# PuduLangHttpClient.Domain.Redirect

## Purpose

Which redirects are followed and how the request changes: the method, whether the body is kept, and whether credentials survive.

## Interface

### Signatures

```pudu
export type Hop = { method: Http.Method, uri: Str, keepsBody: Bool, keepsCredentials: Bool }

export fn isRedirect(code: Int) -> Bool

export fn next(method: &Http.Method, code: Int, from: Str, location: Str) -> Option[Hop]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Domain/Uri]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Exchange]].

## Algorithm

1. Only 300, 301, 302, 303, 307, and 308 redirect, and only to an http or https address.
2. The location is escaped and resolved against the request's address, without its fragment.
3. 303 turns every method but `HEAD` into `GET`; 300, 301, and 302 turn `POST` into `GET`; 307 and 308 keep method and body.
4. Credentials are kept only when the origin is the same.

## Negative Logic (Prohibited Paths)

- A redirect from https to http is never followed.

## Edge Cases

- An empty location is not followed; the redirect response is answered as it is.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why refuse https-to-http redirects?
  **A:** Following one would send the request, and whatever it carries, in the clear to a destination the caller never named. _Rejected:_ following every redirect.

## Referenced by

[[decisions/ADR-0007-secure-defaults]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Domain/_MOC]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Exchange]]
