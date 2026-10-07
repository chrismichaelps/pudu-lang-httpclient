---
type: handoff
from_role: Forensic Guardian
to_role: Architect
status: complete
tags: [handoff, delivery]
---

# Initial package

## Done

- Issue #1 is the ready issue; `feature/1-initial-httpclient-package` is branched from `dev`, which
  is branched from the `main` baseline.
- Every module under `src/` has its mirrored page with a resolved Grill Log ([[src/_MOC]]).
- Against the published 0.1.3 compiler: `pudu check`, `pudu fmt --check`, and `pudu lint` are clean
  over `src`, `test`, `tools`, and `examples`; 41 suites pass with 434 assertions; every example
  answers 0; the mutation gate kills 178 of 178 mutants of `Domain/` ([[architecture/TESTING]]).
- PR #2 into `dev` and PR #3 into `main` passed the Linux `checks` and `mutation` jobs.
- Released 0.1.0 as tag `v0.1.0` with a GitHub release; `pudu search` lists
  `@chrismichaelps/pudu-lang-httpclient`, and a fresh project installs it and runs against it.
- The GitHub wiki holds the API book: eleven chapters whose programs were checked and run against
  the package, a reference index, and the worked programs.
- A compiler issue found during the work is reported upstream as pudu-lang#442.

## Decided (do not re-litigate)

- [[decisions/ADR-0001-outcomes-not-exceptions]] · [[decisions/ADR-0002-owned-transport]] ·
  [[decisions/ADR-0003-factory-owned-pipelines]] · [[decisions/ADR-0004-deadline-cancellation]] ·
  [[decisions/ADR-0005-integrations-at-the-edge]] · [[decisions/ADR-0006-internal-event-bus]] ·
  [[decisions/ADR-0007-secure-defaults]].
- `main` receives the package only once the pull request into `dev` is merged with green checks.

## Open / Remaining

- None for the initial package.

## Exact next action

None; the initial package is released.

## Links

[[00-INDEX]] · [[architecture/TESTING]] · [[CHANGELOG]]

## Referenced by

[[CHANGELOG]] · [[handoffs/_MOC]]
