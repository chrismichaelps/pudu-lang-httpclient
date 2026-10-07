<p align="center">
  <img src="public/pudu-lang-short.png" alt="Pudu" width="120">
</p>

<p align="center">
  <a href="https://www.pudu-lang.org/">Pudu</a> |
  <a href="https://www.pudu-lang.org/docs">Documentation</a> |
  <a href="https://www.pudu-lang.org/packages">Packages</a> |
  <a href="https://github.com/chrismichaelps/pudu-lang-httpclient/wiki">API docs</a> |
  <a href="CONTRIBUTING.md">Contributing</a>
</p>

# pudu-lang-httpclient

Named HTTP clients for Pudu. A factory holds the configuration of every client a program uses —
base address, default headers, timeout, delegating handlers, and the transport underneath — and
hands out cheap clients over handler pipelines it owns, pools, and renews. Every request answers an
`Outcome`: the response, or a `Failure` saying why there is none. Nothing is thrown.

```pudu
import PuduLangHttpClient.Client as Client
import PuduLangHttpClient.Factory as Factory
import PuduLangHttpClient.Factory.Builder as Builder

fn main() -> Int {
  let factory = match Factory.build([
      Builder.defaults().withDefaultHeader("user-agent", "shop/1.0"),
      Builder.named("github").withBaseAddress("https://api.github.com/").withDefaultHeader("accept", "application/vnd.github+json").withTimeout(10000)
    ]) {
    case Ok(built) => built
    case Err(invalid) => panic(Factory.explain(&invalid))
  }
  let client = Factory.createClient(&factory, "github")
  match Client.getText(&client, "repos/chrismichaelps/pudu-lang") {
    case Ok(_) => 0
    case Err(_) => 1
  }
}
```

## Installing

```bash
pudu install @chrismichaelps/pudu-lang-httpclient
```

It needs [Pudu 0.1.3 or later](https://www.pudu-lang.org/download). Every module the package ships
is `PuduLangHttpClient` or under it, so it takes no module name from the program that installs it.

## Concepts

| Concept | Module | What it is |
| --- | --- | --- |
| Outcome and failure | `PuduLangHttpClient` | `Outcome[T]` is `Result[T, Failure]`. A failure names its cause: an invalid request, name resolution, connection, secured connection, proxy tunnel, protocol, a response cut short, a head or body over its limit, too many redirects, a refused destination, an unsupported version, a timeout, a cancellation, an unsuccessful status, an unreadable body, a resilience rejection, or a crash. |
| Requests and responses | `Request`, `Response`, `Content`, `Options` | Verbs, headers, versions with a policy, typed options for handlers, bodies of text, bytes, forms, multipart parts, JSON documents, any value that derives `Json.Encode`, or a producer streamed in chunks; `ensureSuccess`, readers, trailers, and `retryAfter`. |
| Client | `Client`, `Client.Json` | `create`, `over` a transport, or `pooled`; base address, default headers, timeout, response buffer limit, default version, `cancelPending`; `send`, `get`, `getText`, `getBytes`, `stream`, `post`, `put`, `patch`, `delete`, `head`, and `getFromJson`, `postAsJson`, `putAsJson`, `patchAsJson`, `deleteFromJson`. |
| Endpoints | `Endpoint` | Declarative endpoints: a method and a template such as `repos/:owner/:name`, filled from arguments and read as a derived value. |
| Factory | `Factory`, `Factory.Builder` | Named clients, defaults for every client, typed clients, the handler factory (`createHandler`), `rotatingHandler`, handler lifetimes, validation that reports every problem at once, and `dispose`. |
| Handlers | `Handler` | `Send` and `Handler`, composed outermost first; `before`, `after`, and your own functions. |
| Transport | `Transport`, `Cookies`, `Proxy` | The primary handler: pooled keep-alive connections, lifetimes, idle timeouts, a per-server limit, a connect timeout, redirects, gzip, cookie jars that save and restore, proxies with tunnels, an address policy, and head and body limits. |
| Events | `Events` | Plain listeners for requests started, completed, and failed, and pipelines created, expired, and closed; every event derives `Json.Encode`. |
| Testing | `Stub` | A primary handler answering from rules and recording every request. |

## One client, configured once, used everywhere

Build a client at startup with its defaults and hand the same value to every part of the program
that needs it. A client is an immutable value; every copy sends through the same pooled transport, so
the parts share connections:

```pudu
let transport = Transport.create(Transport.defaults().withPooledConnectionLifetime(120000))
let http = Client.over(&transport).withBaseAddress("https://shop.test/").withDefaultHeader("user-agent", "shop/1.0").withTimeout(10000)
let catalog = Catalog{http: http}
let orders = Orders{http: http}
```

`Client.pooled(options)` does the first two steps in one call. Build the client once and pass it
on: a function that builds a client each time it is called builds a new pool each time. Give a
long-lived client a pooled connection lifetime so it still sees addresses change. When several
differently configured clients are needed, give each a name in a factory, set what they share with
`Builder.defaults()`, and ask for them by name wherever they are used.

## Configuring a named client

A builder reads in the order decisions are made. Builders of the same name are combined, and
`Builder.defaults()` applies to every client first.

| Builder method | What it sets |
| --- | --- |
| `withBaseAddress`, `withDefaultHeader`, `withTimeout`, `withMaxResponseContentBufferSize`, `withDefaultVersion`, `configureClient` | the client |
| `withHandler`, `withHandlerFactory`, `configureHandlers` | delegating handlers, made afresh for each pipeline, and their final order |
| `withTransport`, `configureTransport`, `withPrimary` | the primary handler: a pooled transport and its options, or your own |
| `withHandlerLifetime` | how long a pipeline is reused before the next is built (two minutes; `Defaults.INFINITE` to keep it) |
| `observedBy`, `withoutObservers`, `redactingHeaders`, `redactingWhen` | observers placed outside and inside every handler, and which header values they never see |

## Pipelines and lifetimes

A request passes through the observers' outer handlers, the added handlers in order, the observers'
inner handlers, and then the primary handler; the response comes back the same way in reverse.

The factory reuses a name's pipeline — and so its pooled connections — until the handler lifetime
runs out, then builds the next one. Clients are cheap: make one where it is needed. A client made
from an expired pipeline keeps working, and the expired pipeline's connections are closed after one
more lifetime. Code that keeps a sender for a long time uses `Factory.rotatingHandler`, or a
transport with `withPooledConnectionLifetime` and an infinite handler lifetime.

## Handlers

| Module | Package | What it adds |
| --- | --- | --- |
| `Handlers.Logging` | `pudu-lang-log` | an observer writing events where a request enters and leaves the pipeline (`PuduLangHttpClient.<name>.LogicalHandler`) and the primary handler (`…ClientHandler`), with redacted headers at the verbose level |
| `Handlers.Resilience` | `pudu-lang-resilience` | `handler`, `selecting` per request, `fromRegistry`, `onTransient`, `standard` (rate limiter, total timeout, retry honouring `retry-after`, circuit breaker, attempt timeout), and `standardHedging` |
| `Handlers.Propagation` | — | headers of the operation in progress copied onto outgoing requests |
| `Handlers.Authorization` | — | basic credentials, or bearer tokens fetched again once when the server answers 401 |
| `Handlers.Metrics` | — | counts by status class, failures, requests in flight, and durations |

## Events

```pudu
let listeners = Events.listeners()
  .onRequestFailed(|event: Events.RequestFailed| audit(event.client, event.failure))
  .onPipelineChanged(|event: Events.PipelineChanged| track(event.client, event.generation))
let factory = Factory.buildWith(&Factory.Options{listeners: listeners}, builders)
```

Listeners are plain functions; the package carries events to them through a mediator of its own,
which no program has to build or import.

## Cancellation and timeouts

A client links the caller's token, its own pending token, and its timeout. Every socket operation
waits at most what remains of the deadline, and the token is checked between phases and while
waiting for a connection. A timeout of the client's own answers `TimedOut(timeout)`; a cancelled
token answers `Cancelled(reason)`.

## Defaults that fail safe

Credentials are dropped when a redirect changes origin, and https is never followed to http. Cookies
are off unless a transport asks for them. Observers never see `authorization`, `cookie`,
`set-cookie`, or `proxy-authorization` values. Response heads and bodies are bounded (64 KiB and
64 MiB) and refused rather than truncated. Programs that send requests to addresses users supply set
`withAddressPolicy(Transport.PublicOnly([]))`.

## Limits

The transport speaks HTTP/1.0 and HTTP/1.1; an HTTP/2 request is sent as 1.1 under the default
`OrLower` policy and refused under `Exact`. Client certificates are not offered, because Pudu 0.1.3
cannot present one.

## Examples

`examples/` holds runnable programs: `QuickStart`, `SharedClient`, `TypedClients`, `Endpoints`,
`Handlers`, `Resilience`, `Pooling`, and `Testing`. Each starts its own local server.

```bash
pudu run examples/QuickStart.pudu
```

## Developing

```bash
pudu install --locked                                 # the log, resilience, and mediator packages
pudu test test                                        # every suite
pudu fmt --check src test tools examples && pudu lint src test tools examples
pudu run tools/Mutate.pudu --domain --suites test/PuduLangHttpClient/Domain --threshold 100
```

The design lives in the [wiki vault](wiki/00-INDEX.md): one page per source file, the decisions
behind the transport and the factory, and the Pudu grammar rules the code follows. The
[API docs](https://github.com/chrismichaelps/pudu-lang-httpclient/wiki) walk through every module.

## License

[Apache License 2.0](LICENSE).
