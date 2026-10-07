---
type: grammar
language: Pudu
version: "0.1.3"
tags: [grammar]
aliases: [Grammar — Pudu, Pudu Grammar]
---

# Grammar — Pudu

The Pudu surface this repository is written against, pinned to compiler `0.1.3` as published in
its release archive. Where this page and the compiler disagree, the compiler wins and this page is
corrected in the same change.

## SDK Discovery Map

| Need | Module | Entry points |
| --- | --- | --- |
| Sockets | `Std.Net` | `connectWithin`, `sendWithin`, `receiveWithin`, `close`, `listenLocal`, `accept` |
| Secured sockets | `Std.Tls` | `upgradeWithin`, `sendWithin`, `receiveWithin`, `closeWithin` |
| HTTP vocabulary | `Std.Http` | `Method`, `Status`, `Version`, `status`, `methodName`, `renderForm`, `basicAuth`, `bearerAuth` |
| HTTP parsing | `Std.Http.Message`, `Std.Http.Safe` | `parseResponse`, `framingOf`, `headerWritable`, `outboundHost` |
| Test servers | `Std.Http.Server`, `Std.Http.Server.Route` | `start`, `run`, `stop`, `routing`, `get`, `post` |
| Compression | `Std.Compress.Gzip` | `decompressWithin`, `compressText` |
| JSON | `Std.Json` | `decode`, `encode`, the `Encode` and `Decode` traits and their derives |
| Threads | `Std.Concurrent` | `start`, `join`, `joinAll`, `sleep` |
| Cancellation | `Std.Concurrent.Cancel` | `token`, `expiring`, `cancel`, `reason`, `remaining`, `check`, `pause` |
| Shared state | `Std.Sync` | `mutex`, `withLock`, `cell`, `get`, `set`, `counter`, `increment` |
| Integer bounds | `Std.Math` | `min`, `max` |
| Tests | `Std.Test` | `suite`, `equals`, `that`, `not`, `run`, `failuresOf`, `report` |

## Imports / Namespaces

- One module per file; the module name is the path under its source root with `/` as `.`.
- Every import is qualified and aliased. The package root is imported `as HttpClient`.
- Suites import package modules through the manifest's source root and shared helpers from
  `test/Support/`; dependencies resolve from `deps/` after `pudu install --locked`.

## Core Primitives

- Records, sum types, generic aliases of function types, and `Type{..base, field: value}` updates.
- `derives Json.Encode, Json.Decode` writes a type's JSON implementations; events, cookies, metric
  snapshots, and statistics derive them instead of hand-writing encoders.
- A closure captures a copy of every binding it names; a `Sync.Cell` copied this way is still the
  same cell. State shared by threads lives in a cell guarded by a mutex
  ([[src/PuduLangHttpClient/Utils/Shared]]).
- Builders are trait chains: each method answers a changed copy, so configuration reads flat.
- Module scope holds only `const`, folded at compile time.

## Architectural Laws

- Dependency direction is inward: `Domain/` uses only `Constants` and the standard library and
  performs no effects.
- Failures are values: every handler answers an `Outcome`.
- Every module the package ships is `PuduLangHttpClient` or lives under `src/PuduLangHttpClient/`.
- Only `Handlers/Logging`, `Handlers/Resilience`, and `Events/Bus` import other packages.

## Syntax Rules / Naming

- Types, traits, modules, and variants are `PascalCase`; values `camelCase`; constants
  `UPPER_SNAKE_CASE`.
- Every file header and exported type carries the FMCF anchor, one line:
  `/** @Namespace.Entity.Role — intent */`.
- Every `fn`, trait member, and `const` carries a `///` doc comment stating what it answers or
  holds. Rationale belongs in the mirrored page's Grill Log.

## Prohibited Patterns (verified against the 0.1.3 compiler)

- **A brace inside a string literal is interpolation**; a literal brace is `\{` or `\}`. Endpoint
  templates therefore use `:name` slots, as the standard router does.
- **A socket receive that times out closes the connection**; never poll a socket with short reads.
- **Comparing two tuples with `<` checks but fails at run time** (`E7001`, reported upstream as
  pudu-lang#442); compare a single integer key.
- **A tuple is indexed with `[n]`, not `.n`.**
- **`Result` and `Option` have no methods**; use `Result.map`, `Option.unwrapOr`, and so on.
- **A parameter or binding named like a function or import of the module shadows it** (`W2001`).
- **A match arm that only rebuilds its failure is reported** (`W3003`); use `?`.
- **A comparison that picks the smaller or larger of two values is `Math.min` or `Math.max`**, not
  an `if`: the boundary cannot change the answer and mutation testing reports it as a survivor.
- **`items[i]` stops the program when `i` is out of range.**

## Senior Definition Needed

(none open)

## Referenced by

[[00-INDEX]] · [[architecture/_MOC]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Client/Json]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Constants/Events]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Cookies]] · [[src/PuduLangHttpClient/Domain/Chunked]] · [[src/PuduLangHttpClient/Domain/Cookie]] · [[src/PuduLangHttpClient/Domain/Date]] · [[src/PuduLangHttpClient/Domain/Expiry]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Domain/Persistence]] · [[src/PuduLangHttpClient/Domain/Redirect]] · [[src/PuduLangHttpClient/Domain/RetryAfter]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Domain/Version]] · [[src/PuduLangHttpClient/Endpoint]] · [[src/PuduLangHttpClient/Events]] · [[src/PuduLangHttpClient/Events/Bus]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Factory/Entry]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Handlers/Authorization]] · [[src/PuduLangHttpClient/Handlers/Logging]] · [[src/PuduLangHttpClient/Handlers/Metrics]] · [[src/PuduLangHttpClient/Handlers/Propagation]] · [[src/PuduLangHttpClient/Handlers/Resilience]] · [[src/PuduLangHttpClient/Options]] · [[src/PuduLangHttpClient/Proxy]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Stub]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Abort]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Pool]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Transport/Writer]] · [[src/PuduLangHttpClient/Utils/Shared]] · [[src/PuduLangHttpClient/Utils/Template]] · [[src/PuduLangHttpClient/Utils/Tokens]] · [[tools/Mutate]]
