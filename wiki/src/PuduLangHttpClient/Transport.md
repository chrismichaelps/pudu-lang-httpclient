---
type: module
path: "@root/src/PuduLangHttpClient/Transport.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.9
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangHttpClient.Transport]
---

# PuduLangHttpClient.Transport

## Purpose

The primary handler: pooled keep-alive connections, redirects, cookies, decompression, proxies, an address policy, and response limits.

## Interface

### Signatures

```pudu
export type AddressPolicy = AnyAddress | PublicOnly(Array[Str])

export type Options = {
  pooledConnectionLifetime: Int,
  pooledConnectionIdleTimeout: Int,
  maxConnectionsPerServer: Int,
  connectTimeout: Int,
  allowAutoRedirect: Bool,
  maxAutomaticRedirections: Int,
  automaticDecompression: Bool,
  useCookies: Bool,
  cookies: Option[Cookies.Jar],
  proxy: Option[Proxy.Proxy],
  addressPolicy: AddressPolicy,
  maxResponseHeadersLength: Int,
  maxResponseContentLength: Int
}

export type Transport = { options: Options, pool: Pool.Pool, jar: Option[Cookies.Jar] }

export fn defaults() -> Options

export fn validate(options: &Options) -> Array[Str]

export fn create(options: Options) -> Transport

export fn send(transport: &Transport) -> Handler.Send

export fn bufferLimit() -> RequestOptions.Key[Int]

export fn statistics(transport: &Transport) -> Pool.Statistics

export fn cookies(transport: &Transport) -> Option[Cookies.Jar]

export fn close(transport: &Transport) -> ()

export fn isClosed(transport: &Transport) -> Bool

export trait Configuring {
  fn withPooledConnectionLifetime(self: &Self, millis: Int) -> Self
  fn withPooledConnectionIdleTimeout(self: &Self, millis: Int) -> Self
  fn withMaxConnectionsPerServer(self: &Self, count: Int) -> Self
  fn withConnectTimeout(self: &Self, millis: Int) -> Self
  fn withoutRedirects(self: &Self) -> Self
  fn withMaxRedirects(self: &Self, count: Int) -> Self
  fn withDecompression(self: &Self) -> Self
  fn usingCookies(self: &Self) -> Self
  fn withCookies(self: &Self, jar: Cookies.Jar) -> Self
  fn withProxy(self: &Self, proxy: Proxy.Proxy) -> Self
  fn withAddressPolicy(self: &Self, policy: AddressPolicy) -> Self
  fn withMaxResponseHeadersLength(self: &Self, bytes: Int) -> Self
  fn withMaxResponseContentLength(self: &Self, bytes: Int) -> Self
}
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Constants/Messages]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Cookies]], [[src/PuduLangHttpClient/Domain/Headers]], [[src/PuduLangHttpClient/Domain/Persistence]], [[src/PuduLangHttpClient/Domain/Redirect]], [[src/PuduLangHttpClient/Domain/Uri]], [[src/PuduLangHttpClient/Domain/Version]], [[src/PuduLangHttpClient/Handler]], [[src/PuduLangHttpClient/Options]], [[src/PuduLangHttpClient/Proxy]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Transport/Connect]], [[src/PuduLangHttpClient/Transport/Exchange]], [[src/PuduLangHttpClient/Transport/Hop]], [[src/PuduLangHttpClient/Transport/Pool]], [[src/PuduLangHttpClient/Transport/Wire]], [[src/PuduLangHttpClient/Transport/Writer]], [[src/PuduLangHttpClient/Utils/Template]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Factory]], [[src/PuduLangHttpClient/Factory/Builder]], [[src/PuduLangHttpClient/Factory/Entry]].

## Algorithm

1. Each hop checks the token, the endpoint, and the address policy, prepares cookies and `accept-encoding`, routes, writes, and exchanges through [[src/PuduLangHttpClient/Transport/Hop]].
2. A redirect is followed through [[src/PuduLangHttpClient/Domain/Redirect]] up to the limit; credentials are dropped across origins and cookies re-added from the jar.
3. A gzip body is decompressed within the buffer limit when decompression is on.
4. A stalled operation is a cancellation when the token was cancelled and a timeout otherwise.

## Negative Logic (Prohibited Paths)

- A version the transport does not speak, an unwritable header, or an address that is not http or https is refused before anything is sent.

## Edge Cases

- A closed transport still answers, opening connections it closes afterwards.

## Depth

DEPTH 0.9 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why own the transport instead of using the standard client?
  **A:** The standard client opens and closes a connection per request; pooling, lifetimes, per-server limits, proxies, and streaming need control of each connection. _Rejected:_ `Std.Http.Client` ([[decisions/ADR-0002-owned-transport]]).
- **Q:** Why any address by default instead of public addresses only?
  **A:** Named clients usually target services the program was configured with, often on private networks; programs that send requests to addresses users supply opt into `PublicOnly`. _Rejected:_ `PublicOnly` by default.

## Referenced by

[[architecture/_MOC]] · [[decisions/ADR-0002-owned-transport]] · [[domain/Connection]] · [[seams/Primary]] · [[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Cookies]] · [[src/PuduLangHttpClient/Domain/Headers]] · [[src/PuduLangHttpClient/Domain/Persistence]] · [[src/PuduLangHttpClient/Domain/Redirect]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Domain/Version]] · [[src/PuduLangHttpClient/Factory]] · [[src/PuduLangHttpClient/Factory/Builder]] · [[src/PuduLangHttpClient/Factory/Entry]] · [[src/PuduLangHttpClient/Handler]] · [[src/PuduLangHttpClient/Options]] · [[src/PuduLangHttpClient/Proxy]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/Transport/Exchange]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Pool]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Transport/Writer]] · [[src/PuduLangHttpClient/Utils/Template]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Transport]]
