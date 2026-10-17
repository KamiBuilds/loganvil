# Development status

Updated: 2026-10-17.

## Public distribution

This remains a documentation/planning repository. No application source, installable package, executable container or GitHub Release is distributed here. Local tests and a scoped review do not replace the full source-publication gate.

## Verified internal progress

The internal SQLite/FastAPI tracer slice provides bounded JSON/NDJSON ingestion, API-key checks, basic time-windowed search, restrictive SQLite paths, a time-ordering index and a cooperative search execution budget. Earlier validation rejects invalid Unicode attributes and ingestion timestamps that cannot be represented in UTC.

The latest bounded slice moves synchronous SQLite ingestion persistence into the framework worker pool and awaits completion before acknowledging events. Real-lock JSON and NDJSON regressions were observed failing before the fix, then passing: the event loop and health request remained responsive while persistence waited, and acknowledgement followed successful persistence. A real database-abort compatibility control verifies batch rollback, failure without acknowledgement and subsequent recovery; it is not claimed as an additional historical RED-GREEN cycle.

Verification passed:

- 130 automated tests, with one unchanged upstream Starlette test-client deprecation warning; no warnings suppressed.
- Ruff lint/format, strict mypy for 11 source files, compilation, frozen-lock validation, wheel/source-distribution build and diff checks.
- Frozen exported dependency audit: no known vulnerabilities. This does not audit the unpublished application package through PyPI.
- Added-line security scan: no flagged patterns. Secret scan: the same six reviewed documentation/test-fixture findings, none introduced.
- Fresh independent scoped security/correctness review, including a full-suite/static-check rerun. This is not source-publication approval.

An isolated uv environment installed the built wheel with frozen dependencies. Three focused scheduling/rollback cases passed from outside the source checkout, with package/application imports confirmed inside that environment. A real browser exercised the installed wheel on a temporary loopback-only server: health `200`, unauthenticated search `401` and JSON ingest `202`. While a separate connection held an exclusive SQLite lock, NDJSON ingestion remained pending and health returned `200`. Releasing the lock yielded `202` for both events; search returned all three stored events. The owned lock holder exited and the owned server was stopped and verified unreachable.

Linux x86_64/Python 3.11.16 only was exercised, with temporary local SQLite and no GPU. This is isolated wheel evidence, not clean-machine native-install acceptance. Arm64, Docker runtime, resource profiles, ClickHouse, production load, long retention and distributed scale remain unverified.

## Limits and source-publication blockers

Persistence isolation is not whole-request isolation or a load guarantee: parsing, validation and normalization still run on the event loop. Workers share capacity with synchronous routes/dependencies; saturation can delay health and other requests. No ingestion deadline or overload admission control was added. The existing search budget remains cooperative, not a hard HTTP wall-clock deadline.

Publication remains held for:

- Broader cancellation/query-budget acceptance, including explicit client-disconnect cancellation and adversarial search-bound datetime review.
- Safe local-evaluation Compose exposure and actual startup, volume ownership and persistence verification. Docker is unavailable in the current test environment.
- A suitable private security-reporting process before an installable release.
- Fresh full independent publication review after the blockers close.

Week 1 remains in progress. Recovery of this daily slice is complete; no additional milestone was started. Next bounded daily slice: localhost-only local-evaluation Compose binding and reverse-proxy guidance with failing-before-fix configuration regressions. Actual container acceptance remains a separate blocked gate.

## Future capabilities

ClickHouse integration, browser UI, live tail, identity/RBAC, dashboards, alerts, retention, signed distribution and scale/failure validation are incomplete or unverified. See the roadmap for the intended sequence. The 10,000,000+ registered-identity goal remains an unverified distributed architectural target, not a current support claim.

## Update cadence

After each normal daily development slice, publish concise evidence-backed status/roadmap changes when verified state changes. Weekly reports summarize progress and risks. Never publish secrets, private logs, prompts, raw transcripts, unsupported claims or broken implementation to satisfy a cadence.
