---
type: module
path: "@root/src/PuduLangHttpClient/Constants/Messages.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.4
depth_status: SHALLOW
tags: [module, leaf]
aliases: [PuduLangHttpClient.Constants.Messages]
---

# PuduLangHttpClient.Constants.Messages

## Purpose

Every sentence the package reports, as numbered templates.

## Interface

### Signatures

```pudu
export const INVALID_REQUEST: Str

export const NAME_RESOLUTION: Str

export const CONNECTION: Str

export const SECURE_CONNECTION: Str

export const PROXY_TUNNEL: Str

export const PROTOCOL: Str

export const RESPONSE_ENDED: Str

export const CONTENT_TOO_LARGE: Str

export const HEADERS_TOO_LARGE: Str

export const UNFILLED_SLOT: Str

export const TOO_MANY_REDIRECTS: Str

export const NOT_PERMITTED: Str

export const VERSION_UNSUPPORTED: Str

export const TIMED_OUT: Str

export const CANCELLED: Str

export const UNSUCCESSFUL: Str

export const UNREADABLE: Str

export const REJECTED: Str

export const CRASHED: Str

export const NOT_TEXT: Str

export const NOT_JSON: Str

export const NOT_DECODABLE: Str

export const RELATIVE_WITHOUT_BASE: Str

export const NOT_HTTP: Str

export const BAD_HEADER: Str

export const BAD_STATUS_LINE: Str

export const BAD_HEAD: Str

export const BAD_CHUNK: Str

export const BAD_ENCODING: Str

export const STALLED: Str

export const CLIENT_PROBLEM: Str

export const FACTORY_INVALID: Str

export const PROBLEM_SEPARATOR: Str
```

### Linkage

- **Requires:** nothing.
- **Consumed by:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Endpoint]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Exchange]].

## Algorithm

1. Each constant is a template whose `<n>` slots [[src/PuduLangHttpClient/Utils/Template]] fills.

## Negative Logic (Prohibited Paths)

- No sentence is built inline in another module.

## Edge Cases

- A template whose slot has no value keeps the slot as written.

## Depth

DEPTH 0.4 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why one module for every sentence?
  **A:** The wording of failures is part of the interface; one place keeps it consistent and reviewable. _Rejected:_ messages spread across modules.

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/_MOC]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Endpoint]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Exchange]]
