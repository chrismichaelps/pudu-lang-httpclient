---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0007 — Defaults that fail safe

## Context

Clients forward credentials, store cookies, follow redirects, and log headers; each can leak what
the caller meant for one destination.

## Decision

- Credentials (`authorization`, `cookie`, `proxy-authorization`) are dropped when a redirect changes origin, and an https-to-http redirect is never followed ([[src/PuduLangHttpClient/Domain/Redirect]]).
- Cookies are off unless a transport asks for them ([[src/PuduLangHttpClient/Cookies]]).
- Observers never see the values of sensitive headers ([[src/PuduLangHttpClient/Factory/Builder]]).
- Response heads and bodies are bounded (64 KiB and 64 MiB), and refused rather than truncated.
- Header values with line breaks are refused before anything is sent.

## Consequences

- A program that needs cookies, larger bodies, or public-only destinations says so.

## Rejected

- Cookies on by default: a pooled transport would share them between every caller of its name.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[handoffs/2026-10-06-initial-package]]
