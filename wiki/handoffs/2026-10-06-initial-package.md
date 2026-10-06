---
type: handoff
from_role: Forensic Guardian
to_role: Architect
status: in-progress
tags: [handoff, delivery]
---

# Initial package

## Done

- Issue #1 is the ready issue; `feature/1-initial-httpclient-package` is branched from `dev`, which
  is branched from the `main` baseline.
- Every module under `src/` has its mirrored page with a resolved Grill Log ([[src/_MOC]]).
- Suites, examples, and the mutation gate as described in [[architecture/TESTING]].

## Decided (do not re-litigate)

- [[decisions/ADR-0001-outcomes-not-exceptions]] · [[decisions/ADR-0002-owned-transport]] ·
  [[decisions/ADR-0003-factory-owned-pipelines]] · [[decisions/ADR-0004-deadline-cancellation]] ·
  [[decisions/ADR-0005-integrations-at-the-edge]] · [[decisions/ADR-0006-internal-event-bus]] ·
  [[decisions/ADR-0007-secure-defaults]].
- `main` receives the package only once the pull request into `dev` is merged with green checks.

## Open / Remaining

- Open the pull request into `dev`, then release 0.1.0 from `main`.

## Exact next action

Open the pull request from `feature/1-initial-httpclient-package` into `dev`.

## Links

[[00-INDEX]] · [[architecture/TESTING]] · [[CHANGELOG]]

## Referenced by

[[handoffs/_MOC]]
