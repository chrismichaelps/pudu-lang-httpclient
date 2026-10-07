---
type: changelog
tags: [changelog]
---

# Changelog

## 2026-10-07 — Placeholder service suite and mid-request cancellation (#5)

- An explicit cancellation now closes the connection of a request in flight, waking a blocked read at
  once ([[src/PuduLangHttpClient/Transport/Abort]], [[decisions/ADR-0004-deadline-cancellation]]).
- Cancellations report the caller's own reason through the resilience handler and the stub.
- A replica of the public placeholder REST service and suites exercising every CRUD verb on every
  resource through the client, plus an opt-in live contract check ([[architecture/TESTING]]).

## 2026-10-06 — Initial release 0.1.0 (#1)

- Released 0.1.0 as tag `v0.1.0` with a GitHub release; `pudu search` lists the package, and the
  GitHub wiki holds the API book ([[handoffs/2026-10-06-initial-package]]).

## 2026-10-06 — Initial package (#1)

- The package `@chrismichaelps/pudu-lang-httpclient` 0.1.0 with the module root `PuduLangHttpClient`
  and the language range `>=0.1.3 <0.2.0`.
- Failures as values ([[src/PuduLangHttpClient]], [[decisions/ADR-0001-outcomes-not-exceptions]]);
  requests, responses, bodies with derived JSON, streamed uploads, typed options
  ([[subsystems/Messages]]).
- A pooled keep-alive transport with redirects, cookies, gzip, proxies and tunnels, an address
  policy, limits, and stale-connection retry ([[subsystems/Transport]],
  [[decisions/ADR-0002-owned-transport]]).
- Clients with base addresses, default headers, timeouts, and pending cancellation; declarative
  endpoints; a factory of named and typed clients with defaults, handler lifetimes, rotation, and
  closing of retired pipelines ([[subsystems/Factory]], [[decisions/ADR-0003-factory-owned-pipelines]],
  [[decisions/ADR-0004-deadline-cancellation]]).
- Logging, resilience, propagation, authorization, and metrics handlers
  ([[subsystems/Handlers]], [[decisions/ADR-0005-integrations-at-the-edge]]).
- Request and pipeline events heard through plain listeners, carried by a mediator the package owns
  ([[domain/Events]], [[decisions/ADR-0006-internal-event-bus]]); safe defaults
  ([[decisions/ADR-0007-secure-defaults]]).
- Suites for every module, a package layout test, a vault test, an integration scenario, runnable
  examples, and mutation testing of the pure layer ([[architecture/TESTING]]).

## Referenced by

[[00-INDEX]] · [[handoffs/2026-10-06-initial-package]]
