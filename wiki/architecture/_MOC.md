---
type: moc
tags: [moc, architecture]
aliases: [Architecture]
---

# Architecture

## Shape

A program describes each named client with [[src/PuduLangHttpClient/Factory/Builder|builders]]
and builds a [[src/PuduLangHttpClient/Factory|factory]], which reports every configuration problem
at once. Asking the factory for a client hands back a cheap [[src/PuduLangHttpClient/Client|client]]
over the name's current [[src/PuduLangHttpClient/Factory/Entry|handler pipeline]]: observers and
delegating [[src/PuduLangHttpClient/Handler|handlers]] composed around a primary handler, usually a
pooled [[src/PuduLangHttpClient/Transport|transport]]. Pipelines are reused until their lifetime
runs out, then rebuilt; retired pipelines are closed after a grace period.

Every layer answers an `Outcome`, so a refusal by one layer is a value the layers outside it judge
like any other ([[decisions/ADR-0001-outcomes-not-exceptions]]).

## Layers

| Layer | Holds | May import |
| --- | --- | --- |
| `Constants/` | messages, defaults, log templates | nothing |
| `Utils/` | templates, shared state, linked tokens | std, Constants |
| `Domain/` | addresses, headers, chunked decoding, dates, cookies, redirects, persistence, expiry, versions | std, Constants, each other |
| messages | `Options`, `Content`, `Request`, `Response`, `Handler`, `Cookies`, `Proxy` | Domain, Utils, Constants, std |
| `Transport/` | wire, writer, exchange, pool, connect, hop, and the transport | messages, Domain, Utils, std |
| client and factory | `Client`, `Client/Json`, `Endpoint`, `Stub`, `Factory/`, `Events` | everything above |
| `Events/Bus` | the mediator the package owns | `pudu-lang-mediator`, Events, Factory.Builder |
| `Handlers/` | logging, resilience, propagation, authorization, metrics | public modules, `pudu-lang-log`, `pudu-lang-resilience` |

`Domain/` performs no effects. Only `Handlers/Logging`, `Handlers/Resilience`, and `Events/Bus`
import other packages ([[decisions/ADR-0005-integrations-at-the-edge]],
[[decisions/ADR-0006-internal-event-bus]]).

## Pages

- [[architecture/LANGUAGE]] — the vocabulary every page uses.
- [[architecture/TESTING]] — the test levels and what each proves.
- [[grammar/pudu]] — the language rules the code follows.

## Referenced by

[[00-INDEX]]
