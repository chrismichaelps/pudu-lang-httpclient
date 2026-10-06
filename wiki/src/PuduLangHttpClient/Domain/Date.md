---
type: module
path: "@root/src/PuduLangHttpClient/Domain/Date.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, domain]
aliases: [PuduLangHttpClient.Domain.Date]
---

# PuduLangHttpClient.Domain.Date

## Purpose

HTTP dates read as milliseconds since the Unix epoch, in the preferred, obsolete, and C library forms.

## Interface

### Signatures

```pudu
export fn parse(text: Str) -> Option[Int]
```

### Linkage

- **Requires:** nothing.
- **Consumed by:** [[src/PuduLangHttpClient/Domain/Cookie]], [[src/PuduLangHttpClient/Domain/RetryAfter]].

## Algorithm

1. Tokens are split at spaces, commas, and dashes; a token with colons is the time, a known month name is the month, a number of one or two digits is the day and any other number the year.
2. Two-digit years below 70 are in the 2000s.
3. Days are counted from 1970-01-01 in the proleptic Gregorian calendar.

## Negative Logic (Prohibited Paths)

- A date missing any part, a day outside 1-31, a time outside 23:59:60, or a year before 1601 is `None`.

## Edge Cases

- A leap second (`:60`) is accepted and counted as the following second.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a hand-written reader instead of `Std.Time.parse`?
  **A:** HTTP dates come in three forms and cookies add a dashed variant; one tolerant reader covers all, and as pure arithmetic it is mutation-tested. _Rejected:_ `Std.Time.parse` with one pattern per form.

## Referenced by

[[src/PuduLangHttpClient/Domain/Cookie]] · [[src/PuduLangHttpClient/Domain/RetryAfter]] · [[src/PuduLangHttpClient/Domain/_MOC]]
