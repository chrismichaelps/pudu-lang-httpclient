---
type: module
path: "@root/src/PuduLangHttpClient/Proxy.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Proxy]
---

# PuduLangHttpClient.Proxy

## Purpose

A forward proxy: its address, credentials, and the hosts reached around it.

## Interface

### Signatures

```pudu
export type Proxy = { address: Str, credentials: Option[(Str, Str)], bypass: Array[Str], bypassOnLocal: Bool }

export fn at(address: Str) -> Proxy

export fn validate(proxy: &Proxy) -> Array[Str]

export fn bypasses(proxy: &Proxy, host: Str) -> Bool

export fn authorization(proxy: &Proxy) -> Option[Str]

export trait Configuring {
  fn withCredentials(self: &Self, user: Str, password: Str) -> Self
  fn bypassing(self: &Self, hosts: &Array[Str]) -> Self
  fn bypassingLocal(self: &Self) -> Self
}
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient/Domain/Uri]], the standard library.
- **Consumed by:** [[src/PuduLangHttpClient/Transport]], [[src/PuduLangHttpClient/Transport/Connect]].

## Algorithm

1. `bypasses` checks the listed hosts, `*.suffix` entries, and, when asked, local hosts and names without a dot.
2. `authorization` writes the credentials as a basic value.

## Negative Logic (Prohibited Paths)

- An https proxy address is refused: the transport speaks to proxies in the clear and tunnels secured targets through them.

## Edge Cases

- Credentials are sent to the proxy only, never to the target.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why tunnel secured targets with `CONNECT`?
  **A:** The proxy then relays bytes it cannot read, and the target's certificate is verified end to end. _Rejected:_ sending secured requests to the proxy in absolute form.

## Referenced by

[[src/PuduLangHttpClient/Domain/Uri]] · [[src/PuduLangHttpClient/Transport]] · [[src/PuduLangHttpClient/Transport/Connect]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Transport]]
