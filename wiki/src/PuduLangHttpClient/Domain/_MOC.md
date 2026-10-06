---
type: moc
tags: [moc]
---

# PuduLangHttpClient.Domain

- [[src/PuduLangHttpClient/Domain/Chunked]] — A chunked body decoded incrementally as its bytes arrive, with trailers and leftover bytes kept, bounded by a limit on decoded bytes.
- [[src/PuduLangHttpClient/Domain/Cookie]] — Cookies read from `set-cookie` values and matched to requests: domains, paths, security, expiry, storage, and rendering.
- [[src/PuduLangHttpClient/Domain/Date]] — HTTP dates read as milliseconds since the Unix epoch, in the preferred, obsolete, and C library forms.
- [[src/PuduLangHttpClient/Domain/Expiry]] — Whether a lifetime or idle limit has run out, what remains of a deadline, and the earlier of two deadlines.
- [[src/PuduLangHttpClient/Domain/Headers]] — Header lists compared without regard to case: lookups, sets, removals, defaults merged under a message's own headers, redaction, the split between message and content headers, and writability checks.
- [[src/PuduLangHttpClient/Domain/Persistence]] — Which connections stay open after an exchange, and which pooled connections may still be reused.
- [[src/PuduLangHttpClient/Domain/Redirect]] — Which redirects are followed and how the request changes: the method, whether the body is kept, and whether credentials survive.
- [[src/PuduLangHttpClient/Domain/RetryAfter]] — How long a `retry-after` header asks the client to wait: seconds, or the time until an HTTP date.
- [[src/PuduLangHttpClient/Domain/Uri]] — Addresses split into their five components, references resolved against a base address, dot segments removed, and absolute http or https addresses reduced to the endpoint they connect to.
- [[src/PuduLangHttpClient/Domain/Version]] — The protocol version a request is sent with, from the version it asks for and its policy.

## Referenced by

[[src/PuduLangHttpClient/_MOC]]
