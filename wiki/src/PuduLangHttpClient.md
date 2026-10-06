---
type: module
path: "@root/src/PuduLangHttpClient.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangHttpClient]
---

# PuduLangHttpClient

## Purpose

The package root and its vocabulary: the `Failure` a request answers instead of a response, the `Outcome` every handler answers, and the helpers that classify and describe failures.

## Interface

### Signatures

```pudu
export type Failure

export type Outcome[T] = Result[T, Failure]

export fn invalid[T](reason: Str) -> Outcome[T]

export fn isTransient(failure: &Failure) -> Bool

export fn isCancellation(failure: &Failure) -> Bool

export fn isTimeout(failure: &Failure) -> Bool

export fn statusOf(failure: &Failure) -> Option[Int]

export fn describe(failure: &Failure) -> Str
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Constants/Messages]], [[src/PuduLangHttpClient/Utils/Template]].
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Client/Json]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Cookies]], [[src/PuduLangHttpClient/Endpoint]], [[src/PuduLangHttpClient/Events/Bus]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Handlers/Authorization]], [[src/PuduLangHttpClient/Handlers/Logging]], [[src/PuduLangHttpClient/Handlers/Metrics]], [[src/PuduLangHttpClient/Handlers/Propagation]], [[src/PuduLangHttpClient/Handlers/Resilience]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Stub]], [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Exchange]], [[src/PuduLangHttpClient/Transport/Hop]], [[src/PuduLangHttpClient/Transport/Wire]].

## Algorithm

1. `Failure` names every way a request can end without a response, one variant per cause a caller acts on differently.
2. `isTransient` marks the failures a repeat may cure: name resolution, a broken connection or tunnel, a malformed or truncated response, and a timeout.
3. `describe` renders every failure through [[src/PuduLangHttpClient/Constants/Messages]].

## Negative Logic (Prohibited Paths)

- Nothing is thrown; a status code is never a failure until `Response.ensureSuccess` makes it one.

## Edge Cases

- `Unsuccessful` carries the status code and reason so `statusOf` answers it.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a closed set of failures rather than one failure with a message?
  **A:** Callers retry, report, or give up depending on the cause; a variant per cause lets them match instead of parsing text. _Rejected:_ a single failure carrying text (callers would parse messages).
- **Q:** Why is a security failure (`SecureConnection`) not transient?
  **A:** A certificate that does not verify is a connection to somebody unknown; retrying around it is the wrong reflex. _Rejected:_ treating every connection failure alike.

## Referenced by

[[CHANGELOG]] · [[decisions/ADR-0001-outcomes-not-exceptions]] · [[domain/Outcome]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Client/Json]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Cookies]] · [[src/PuduLangHttpClient/Endpoint]] · [[src/PuduLangHttpClient/Events/Bus]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/Authorization]] · [[src/PuduLangHttpClient/Handlers/Logging]] · [[src/PuduLangHttpClient/Handlers/Metrics]] · [[src/PuduLangHttpClient/Handlers/Propagation]] · [[src/PuduLangHttpClient/Handlers/Resilience]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Stub]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Utils/Template]] · [[src/_MOC]]
