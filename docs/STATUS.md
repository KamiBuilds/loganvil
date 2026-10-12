# Development status

Updated: 2026-10-12.

## Public distribution

This is a documentation/planning repository. No application code, installable package, executable container or GitHub Release is currently distributed here. Public source publication is withheld after independent review; a passing existing suite alone does not make an implementation ready.

## Verified internal progress

The current internal SQLite/FastAPI tracer slice provides bounded JSON/NDJSON ingestion, shared API-key checks, basic time-windowed search, restrictive SQLite paths and a time-ordering index. Parent verification passed:

- 61 automated tests, with one upstream Starlette test-client deprecation warning.
- Ruff lint and format checks.
- Strict mypy checks for 11 source files.
- Frozen dependency-lock validation.

Independent review also exercised package build, lock-based dependency auditing and an isolated installed-wheel localhost health/ingest/search smoke test. Linux x86_64/Python 3.11.16 only was exercised; arm64, Docker runtime, production load and distributed scale are unverified.

## Source-publication blockers

- Reject invalid Unicode scalar values in nested event attributes before persistence; the current internal behavior can make a stored event unsearchable.
- Handle timezone normalization at datetime range boundaries as controlled validation failures.
- Add bounded query execution/cancellation and isolate synchronous SQLite work from the asynchronous event loop.
- Make local-evaluation Compose binding safe and verify real startup, volume ownership and persistence.
- Establish a suitable private security-reporting process before an installable release.
- Obtain a fresh independent publication review after all blockers close.

Next bounded slice: Unicode attribute validation with observed failing regressions before the fix. Remaining slices are handled deliberately, not bundled into a rushed release.

## Future capabilities

ClickHouse integration, browser UI, live tail, identity/RBAC, dashboards, alerts, retention, signed distribution and scale/failure validation are incomplete or unverified. See the roadmap for the intended sequence.

## Update cadence

After each normal daily development slice, publish a concise evidence-backed status/roadmap update when state changes. Weekly reports summarize verified progress and risks. Do not publish secrets, private logs, prompts, raw transcripts, unsupported claims or broken implementation to satisfy a cadence.
