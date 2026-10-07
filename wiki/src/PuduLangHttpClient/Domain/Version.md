---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Version.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Version]
---

# PuduLangHttpClient.Domain.Version

## Purpose

The protocol version a request is sent with, from the version it asks for and its policy.

## Interface

### Signatures

```pudu
export type Policy = OrLower | Exact | OrHigher

export fn negotiate(requested: &Http.Version, policy: &Policy) -> Option[Http.Version]
```

### Linkage

- **Requires:** the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Factory/Builder]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Transport]].

## Algorithm

1. The transport speaks HTTP/1.0 and HTTP/1.1.
2. `OrLower` sends the highest of those at or below the request; `Exact` sends only a version spoken; `OrHigher` sends 1.1 for 1.0 or 1.1.

## Negative Logic (Prohibited Paths)

- A version the transport does not speak is never written on the wire.

## Edge Cases

- HTTP/2 and HTTP/3 under `Exact` or `OrHigher` have no version and the request fails with `VersionUnsupported`.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why accept HTTP/2 requests at all?
  **A:** A request may name a version and a policy; under `OrLower` the transport honours it the best it can, which is what a caller that tolerates a downgrade asked for. _Rejected:_ refusing every HTTP/2 request.

## Referenced by

[[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Domain/_MOC]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Transport]]
