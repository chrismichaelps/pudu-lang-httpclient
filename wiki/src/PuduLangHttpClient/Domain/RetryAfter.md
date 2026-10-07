---
type: module
path: "@root/src/PuduLangHttpClient/Domain/RetryAfter.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.RetryAfter]
---

# PuduLangHttpClient.Domain.RetryAfter

## Purpose

How long a `retry-after` header asks the client to wait: seconds, or the time until an HTTP date.

## Interface

### Signatures

```pudu
export fn delay(value: Str, now: Int) -> Option[Int]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Domain/Date]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Response]].

## Algorithm

1. A number is seconds; anything else is read by [[src/PuduLangHttpClient/Domain/Date]] and measured from `now`.

## Negative Logic (Prohibited Paths)

- A negative number asks for nothing.

## Edge Cases

- A date in the past asks for no wait.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why answer `None` instead of 0 for unreadable values?
  **A:** No answer lets the retry strategy keep its own backoff; 0 would retry at once. _Rejected:_ 0 for anything unreadable.

## Referenced by

[[src/PuduLangHttpClient/Domain/Date]] · [[src/PuduLangHttpClient/Domain/_MOC]] · [[src/PuduLangHttpClient/Response]]
