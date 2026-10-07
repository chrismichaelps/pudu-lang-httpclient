---
type: module
path: "@root/src/PuduLangHttpClient/Endpoint.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.5
depth_status: MODERATE
tags: [module, leaf]
aliases: [PuduLangHttpClient.Endpoint]
---

# PuduLangHttpClient.Endpoint

## Purpose

Declarative endpoints: a method and an address template whose `{name}` slots are filled from arguments, sent and read as JSON.

## Interface

### Signatures

```pudu
export type Endpoint = { method: Http.Method, template: Str }

export fn get(template: Str) -> Endpoint

export fn post(template: Str) -> Endpoint

export fn put(template: Str) -> Endpoint

export fn patch(template: Str) -> Endpoint

export fn delete(template: Str) -> Endpoint

export fn fill(endpoint: &Endpoint, arguments: &Array[(Str, Str)]) -> HttpClient.Outcome[Str]

export fn request(client: &Client.Client, endpoint: &Endpoint, arguments: &Array[(Str, Str)]) -> HttpClient.Outcome[Request.Request]

export fn send(client: &Client.Client, endpoint: &Endpoint, arguments: &Array[(Str, Str)]) -> HttpClient.Outcome[Response.Response]

export fn call[T: Json.Decode](client: &Client.Client, endpoint: &Endpoint, arguments: &Array[(Str, Str)]) -> HttpClient.Outcome[T]

export fn callWith[B: Json.Encode, T: Json.Decode](client: &Client.Client, endpoint: &Endpoint, arguments: &Array[(Str, Str)], body: &B) -> HttpClient.Outcome[T]
```

### Linkage

- **Requires:** [[src/PuduLangHttpClient]], [[src/PuduLangHttpClient/Client]], [[src/PuduLangHttpClient/Constants/Messages]], [[src/PuduLangHttpClient/Content]], [[src/PuduLangHttpClient/Request]], [[src/PuduLangHttpClient/Response]], [[src/PuduLangHttpClient/Utils/Template]], the standard library.
- **Consumed by:** the program.

## Algorithm

1. `fill` replaces each `{name}` with its argument, percent-encoded; an unknown slot is refused.
2. `call` sends the endpoint and reads a derived value; `callWith` also writes a derived body.

## Negative Logic (Prohibited Paths)

- A template with an unfilled slot is never sent.

## Edge Cases

- Arguments are encoded as components, so `/` and `?` in a value cannot change the address's shape.

## Depth

DEPTH 0.5 (MODERATE). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why templates in data rather than generated code?
  **A:** Pudu has no attribute-driven code generation for interfaces; a record of method and template is as declarative and is checked when the request is built. _Rejected:_ hand-written methods per endpoint.

## Referenced by

[[src/PuduLangHttpClient]] · [[src/PuduLangHttpClient/Client]] · [[src/PuduLangHttpClient/Constants/Messages]] · [[src/PuduLangHttpClient/Content]] · [[src/PuduLangHttpClient/Request]] · [[src/PuduLangHttpClient/Response]] · [[src/PuduLangHttpClient/Utils/Template]] · [[src/PuduLangHttpClient/_MOC]] · [[subsystems/Factory]]
