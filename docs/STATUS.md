# Development status

Updated: 2026-10-14.

## Public distribution

This is a documentation/planning repository. No application code, installable package, executable container or GitHub Release is currently distributed here. Public source publication remains withheld; passing local checks and a scoped fix review do not replace the full publication gate.

## Verified internal progress

The current internal SQLite/FastAPI tracer slice provides bounded JSON/NDJSON ingestion, shared API-key checks, basic time-windowed search, restrictive SQLite paths and a time-ordering index.

The recovered daily slice now rejects invalid Unicode scalar values in direct and nested attribute keys/values before persistence. Forty sequential RED-GREEN regression cases cover JSON/NDJSON, high/low surrogate boundaries and whole-batch rejection. Valid non-ASCII keys and supplementary characters still survive ingestion and search unchanged. Previously stored malformed attributes are not migrated or repaired by this fix.

Verification passed:

- 101 automated tests, with one unchanged upstream Starlette test-client deprecation warning.
- Ruff lint/format and strict mypy checks for 11 source files.
- Compile checks, frozen dependency-lock validation and wheel/source-distribution build.
- Frozen-lock dependency audit: no known vulnerabilities; the unpublished local package could not be audited through PyPI.
- Added-line security scan: no flagged patterns. The tracked secret scan retained six reviewed documentation/test-fixture findings and introduced no new findings.
- Independent security/correctness review of this Unicode fix, including an independent full-suite rerun. This is not approval for source publication.

An isolated uv environment installed the built wheel with frozen runtime dependencies. Browser verification against a temporary loopback-only server confirmed health, malformed JSON/NDJSON rejection, zero rejected events in search, valid Unicode round-trip and rejection of unauthenticated search. The test server was shut down afterward.

Linux x86_64/Python 3.11.16 only was exercised, with temporary local SQLite files and no GPU. Isolated wheel installation is not clean-machine native installation acceptance. Arm64, Docker runtime, resource profiles, ClickHouse, production load, long retention and distributed scale remain unverified.

## Source-publication blockers

- Handle timezone normalization at datetime range boundaries as controlled validation failures.
- Add bounded query execution/cancellation and isolate synchronous SQLite work from the asynchronous event loop.
- Make local-evaluation Compose binding safe and verify real startup, volume ownership and persistence. Docker is unavailable in the current test environment.
- Establish a suitable private security-reporting process before an installable release.
- Obtain a fresh independent publication review after all blockers close.

Next bounded daily slice: ingestion UTC normalization overflow with observed failing regressions before the fix. Only the interrupted Unicode slice was resumed; no additional milestone was started.

## Future capabilities

ClickHouse integration, browser UI, live tail, identity/RBAC, dashboards, alerts, retention, signed distribution and scale/failure validation are incomplete or unverified. See the roadmap for the intended sequence.

## Update cadence

After each normal daily development slice, publish a concise evidence-backed status/roadmap update when state changes. Weekly reports summarize verified progress and risks. Do not publish secrets, private logs, prompts, raw transcripts, unsupported claims or broken implementation to satisfy a cadence.
