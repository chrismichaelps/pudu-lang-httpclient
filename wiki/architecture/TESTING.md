---
type: architecture
tags: [architecture, test]
aliases: [Testing]
---

# Testing

Every suite is a file under `test/`, mirroring the module it covers; `pudu test test` runs them all
and each suite names its failed checks on stderr. `test/Support/Loopback` starts routed servers and
scripted raw-socket servers on the loopback interface, so no suite needs the network.

| Level | Suites | What they prove |
| --- | --- | --- |
| Domain | `test/PuduLangHttpClient/Domain/**` | every reference-resolution example, chunk boundaries, HTTP dates, redirect rules, cookie matching and storage, persistence, expiry, versions, retry-after |
| Messages | the root suite, `OptionsTest`, `ContentTest`, `RequestTest`, `ResponseTest`, `HandlerTest`, `CookiesTest`, `StubTest`, `EndpointTest` | failures, typed options, bodies and derived JSON, builders, readers, composition order, jars saved and restored, stubbed answers, endpoint templates |
| Transport | `TransportTest`, `Transport/**` | keep-alive reuse, redirects, cookies, gzip, address policy, limits, deadlines, lifetimes, per-server limits, chunked and close-framed bodies, interim responses, stale-connection retry, streamed uploads and downloads, proxies and tunnels, the pool |
| Client and factory | `ClientTest`, `Client/JsonTest`, `FactoryTest`, `Factory/**`, `EventsTest`, `Events/BusTest` | base addresses, defaults, timeouts, pending cancellation, typed clients, builder combination, rotation and grace, lifecycle events, observers |
| Handlers | `test/PuduLangHttpClient/Handlers/**` | log events and redaction, retries, breakers, timeouts, rate limits, registries, propagation, authorization refresh, metrics |
| Utils | `Utils/UtilsTest` | templates, atomic shared updates under threads, linked tokens |
| Package | `test/Package/LayoutTest` | every shipped module is the root `PuduLangHttpClient` or under it, is named after its path, and agrees with the manifest |
| Vault | `test/Package/VaultTest` | the vault mirrors `src/` page for page, every page has a Grill Log, every exported function is in its page's signatures, every link resolves, and every page lists the pages linking to it |
| Integration | `test/Integration/ShopScenarioTest` | one shop through the factory: logging, metrics, listeners, the standard resilience pipeline, authorization, a typed client, and JSON |
| Placeholder service | `test/Integration/Placeholder*Test`, `test/Support/Placeholder/` | a replica of the public placeholder REST service written in Pudu (six resources at the public counts and shapes, nested routes, filters, pagination, sorting, every CRUD verb, fault and latency injection) read and written through a typed client: the contract, every verb on every resource, validation, 25 parallel creates, keep-alive, retries, an opening breaker, timeouts, mid-request cancellation, streaming, limits, rotation, logs, metrics, and events |
| Live contract | `test/Integration/PlaceholderLiveTest` with `PUDU_LIVE=1` | the replica agrees with the public service on counts, owners, records, and writes; offline it checks only the replica |
| Examples | `examples/*.pudu`, run by CI | the documented programs compile and answer 0 |
| Mutation | [[tools/Mutate]] | the domain suites notice single-point changes to the pure layer |

The mutation gate runs on pull requests over `Domain/` against the domain suites with a threshold
of 100: every valid mutant is killed.

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[handoffs/2026-10-06-initial-package]]
