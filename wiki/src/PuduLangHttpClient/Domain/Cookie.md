---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Cookie.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Cookie]
---

# PuduLangHttpClient.Domain.Cookie

## Purpose

Cookies read from `set-cookie` values and matched to requests: domains, paths, security, expiry, storage, and rendering.

## Interface

### Signatures

```pudu
export type Stored = {
  name: Str,
  value: Str,
  domain: Str,
  hostOnly: Bool,
  path: Str,
  secure: Bool,
  httpOnly: Bool,
  expires: Option[Int],
  created: Int
} derives Json.Encode, Json.Decode

export fn parse(header: Str, host: Str, path: Str, now: Int) -> Option[Stored]

export fn domainMatches(host: Str, domain: Str) -> Bool

export fn pathMatches(requestPath: Str, cookiePath: Str) -> Bool

export fn expired(cookie: &Stored, now: Int) -> Bool

export fn applies(cookie: &Stored, host: Str, path: Str, secure: Bool, now: Int) -> Bool

export fn store(jar: &Array[Stored], cookie: &Stored, now: Int) -> Array[Stored]

export fn render(cookies: &Array[Stored]) -> Str

export fn defaultPath(path: Str) -> Str

export fn isPersistent(cookie: &Stored) -> Bool
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Domain/Date]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Cookies]].

## Algorithm

1. `parse` reads the name and value, then `Domain`, `Path`, `Secure`, `HttpOnly`, `Max-Age`, and `Expires`; `Max-Age` wins over `Expires`.
2. A cookie without `Domain` is host-only; one with a domain the host is not in is refused.
3. `store` replaces a cookie of the same name, domain, and path, keeping its creation time; an expired cookie only removes.
4. `render` orders longer paths first, then older cookies.

## Negative Logic (Prohibited Paths)

- No public-suffix list: a server may set a cookie for any domain its host belongs to.

## Edge Cases

- The default path is the request path up to its last segment, or `/`.
- An address host never matches a domain by suffix.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why derive `Json.Encode` and `Json.Decode` on `Stored`?
  **A:** A jar is saved and restored as JSON; deriving keeps the format in step with the record and removes hand-written encoders. _Rejected:_ hand-written encoding.

## Referenced by

[[src/PuduLangHttpClient/Cookies]] · [[src/PuduLangHttpClient/Domain/Date]] · [[src/PuduLangHttpClient/Domain/_MOC]]
