---
type: module
path: "@root/src/PuduLangHttpClient/Transport/Connect.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, leaf]
aliases: [PuduLangHttpClient.Transport.Connect]
---

# PuduLangHttpClient.Transport.Connect

## Purpose

Routes chosen per endpoint and connections opened directly or through a proxy, within a connect timeout.

## Interface

### Signatures

```pudu
export type Route = { key: Str, target: Uri.Endpoint, via: Option[Proxy.Proxy] }

export fn routeTo(target: &Uri.Endpoint, proxy: &Option[Proxy.Proxy]) -> Route

export fn isProxied(route: &Route) -> Bool

export fn open(route: &Route, connectTimeout: Int, deadline: Option[Int]) -> Result[Wire.Link, Wire.Fault]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Constants/Defaults]], [[src/PuduLangHttpClient/Constants/Messages]], [[src/PuduLangHttpClient/Domain/Expiry]], [[src/PuduLangHttpClient/Domain/Uri]], [[src/PuduLangHttpClient/Proxy]], [[src/PuduLangHttpClient/Transport/Wire]], [[src/PuduLangHttpClient/Utils/Template]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Hop]].

## Algorithm

1. A route is direct, through a plain proxy (absolute-form requests), or a tunnel opened with `CONNECT` and secured end to end.
2. Routes key the pool: direct routes by origin, plain proxy routes by proxy, tunnels by proxy and origin.

## Negative Logic (Prohibited Paths)

- A tunnel the proxy refuses is closed and reported as `ProxyTunnel`.

## Edge Cases

- A connect timeout shorter than the request's deadline is reported as a connection failure, not a timeout of the request.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why share plain-proxy connections across origins?
  **A:** Every request goes to the proxy itself, so its connections are interchangeable. _Rejected:_ one pool entry per origin through the proxy.

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Constants/Defaults]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Domain/Expiry]] · [[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Proxy]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Hop]] · [[src/PuduLangHttpClient/Transport/Wire]] · [[src/PuduLangHttpClient/Transport/_MOC]] · [[src/PuduLangHttpClient/Utils/Template]]
