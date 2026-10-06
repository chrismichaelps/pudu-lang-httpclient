---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Uri.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.9
depth_status: DEEP
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Uri]
---

# PuduLangHttpClient.Domain.Uri

## Purpose

Addresses split into their five components, references resolved against a base address, dot segments removed, and absolute http or https addresses reduced to the endpoint they connect to.

## Interface

### Signatures

```pudu
export type Parts = { scheme: Option[Str], authority: Option[Str], path: Str, query: Option[Str], fragment: Option[Str] }

export type Endpoint = { scheme: Str, host: Str, port: Int, secure: Bool }

export fn split(text: Str) -> Parts

export fn join(parts: &Parts) -> Str

export fn isAbsolute(text: Str) -> Bool

export fn resolve(base: Str, reference: Str) -> Str

export fn removeDotSegments(path: Str) -> Str

export fn endpointOf(text: Str) -> Option[Endpoint]

export fn origin(endpoint: &Endpoint) -> Str

export fn authority(endpoint: &Endpoint) -> Str

export fn target(text: Str) -> Str

export fn withoutFragment(text: Str) -> Str

export fn escape(text: Str) -> Str
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Constants/Defaults]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Cookies]], [[src/PuduLangHttpClient/Domain/Redirect]], [[src/PuduLangHttpClient/Proxy]], [[src/PuduLangHttpClient/Stub]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Writer]].

## Algorithm

1. `split` follows the reference grammar's regular split, keeping an absent component apart from an empty one.
2. `resolve` follows the strict reference-resolution algorithm, merging a relative path under the base path's last directory.
3. `removeDotSegments` applies `.` and `..` segments in one pass over the input.
4. `endpointOf` lowers the scheme and host, drops user information, unwraps an IPv6 literal, and fills in the scheme's port.
5. `escape` percent-encodes, as UTF-8, every character a request line cannot carry.

## Negative Logic (Prohibited Paths)

- No decoding: components are kept exactly as written, so nothing is decoded twice.
- Only `http` and `https` have endpoints.

## Edge Cases

- A base address without a trailing slash loses its last segment when a relative path is resolved against it, as the reference algorithm requires.
- A port of 0, above 65535, or not a number has no endpoint.

## Depth

DEPTH 0.9 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why not use `Std.Url` for resolution?
  **A:** `Std.Url` parses the query into pairs and renders it again, which re-encodes it; resolution must keep every component byte for byte. _Rejected:_ `Std.Url.parse` plus rendering (changes the address the caller wrote).
- **Q:** Why escape instead of refusing addresses with spaces?
  **A:** Line breaks and spaces are what break a request line; escaping them makes every address safe to send and keeps the caller's intent. _Rejected:_ refusing the request (rejects addresses that browsers accept).

## Referenced by

[[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Cookies]] · [[src/PuduLangHttpClient/Domain/Redirect]] · [[src/PuduLangHttpClient/Domain/_MOC]] · [[src/PuduLangHttpClient/Proxy]] · [[src/PuduLangHttpClient/Stub]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Writer]]
