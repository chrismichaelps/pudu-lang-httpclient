---
type: module
path: "@root/src/PuduLangHttpClient/Handlers/Resilience.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, leaf]
aliases: [PuduLangHttpClient.Handlers.Resilience]
---

# PuduLangHttpClient.Handlers.Resilience

## Purpose

Requests run through pudu-lang-resilience pipelines tuned for HTTP: one pipeline, one per request, one from a registry, the standard pipeline, and the standard hedging pipeline.

## Interface

### Signatures

```pudu
export type Guarding = Pipeline.Pipeline[Response.Response, HttpClient.Failure]

export type Standard = {
  rateLimiter: RateLimiter.Options,
  totalRequestTimeout: Timeout.Options,
  retry: Retry.Options[Response.Response, HttpClient.Failure],
  circuitBreaker: CircuitBreaker.Options[Response.Response, HttpClient.Failure],
  attemptTimeout: Timeout.Options
}

export type StandardHedging = {
  totalRequestTimeout: Timeout.Options,
  hedging: Hedging.Options[Response.Response, HttpClient.Failure],
  circuitBreaker: CircuitBreaker.Options[Response.Response, HttpClient.Failure],
  attemptTimeout: Timeout.Options
}

export fn handler(pipeline: Guarding) -> Handler.Handler

export fn selecting(choose: fn(Request.Request) -> Guarding) -> Handler.Handler

export fn fromRegistry(registry: &Registry.Registry[Response.Response, HttpClient.Failure], key: Str) -> Handler.Handler

export fn standardOptions() -> Standard

export fn standard() -> Result[Guarding, Pipeline.Invalid]

export fn standardWith(options: &Standard) -> Result[Guarding, Pipeline.Invalid]

export fn standardHedgingOptions() -> StandardHedging

export fn standardHedging() -> Result[Guarding, Pipeline.Invalid]

export fn standardHedgingWith(options: &StandardHedging) -> Result[Guarding, Pipeline.Invalid]

export fn onTransient(build: fn(Predicate.Predicate[Response.Response, HttpClient.Failure]) -> Array[Strategy.Strategy[Response.Response, HttpClient.Failure]]) -> Result[Guarding, Pipeline.Invalid]

export fn transient() -> Predicate.Predicate[Response.Response, HttpClient.Failure]

export fn isTransientStatus(code: Int) -> Bool

export fn retryAfter(given: Predicate.Arguments[Response.Response, HttpClient.Failure]) -> Option[Int]

export fn translated(failure: &Guarded.Failure[HttpClient.Failure]) -> HttpClient.Failure
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], `pudu-lang-resilience`, the standard library.
- **Consumed by:** the program.

## Algorithm

1. An attempt stopped by its own token answers a cancellation so the strategy that fired it can tell; failures are raised to the strategies.
2. `transient` handles 408, 429, every 5xx, transient failures, and timeouts.
3. `retryAfter` lets a handled response's `retry-after` set the delay.
4. `onTransient` builds a pipeline from strategies given the transient predicate.

## Negative Logic (Prohibited Paths)

- A caller's deadline stays a timeout and a caller's cancellation stays a cancellation.

## Edge Cases

- A key the registry cannot build is `Rejected`.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why let results, not only failures, trigger retries?
  **A:** A 503 is a response, not a failure; the handler answers it as one and lets the predicate decide. _Rejected:_ turning every non-success status into a failure first.

## Referenced by

[[decisions/ADR-0005-integrations-at-the-edge]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/_MOC]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[subsystems/Handlers]]
