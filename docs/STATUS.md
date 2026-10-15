# Development status

Updated: 2026-10-15.

## Public distribution

This is a documentation/planning repository. No application code, installable package, executable container or GitHub Release is currently distributed here. Public source publication remains withheld; passing local checks and a scoped fix review do not replace the full publication gate.

## Verified internal progress

The current internal SQLite/FastAPI tracer slice provides bounded JSON/NDJSON ingestion, shared API-key checks, basic time-windowed search, restrictive SQLite paths and a time-ordering index. Earlier work rejects invalid Unicode attribute keys/values before persistence.

The recovered daily slice now rejects ingestion timestamps whose UTC conversion would fall outside the datetime range. Eight JSON/NDJSON regressions first returned server errors, then passed with controlled validation responses, zero persisted rows and successful subsequent ingestion/search. NDJSON coverage includes a valid event before an invalid event. Twelve compatibility controls preserve exact valid UTC endpoints and offset equivalents through SQLite persistence.

Verification passed:

- 121 automated tests, with one unchanged upstream Starlette test-client deprecation warning.
- Ruff lint/format and strict mypy checks for 11 source files.
- Compile checks, frozen dependency-lock validation and wheel/source-distribution build.
- Exported frozen dependency audit: no known vulnerabilities. This does not audit the unpublished application package through PyPI.
- Added-line security scan: no flagged patterns. The tracked secret scan retained six reviewed documentation/test-fixture findings and introduced no new findings.
- Independent security/correctness review of this ingestion fix, including an independent full-suite rerun. This is not approval for source publication.

An isolated uv environment installed the built wheel with frozen runtime dependencies. Outside-source isolated Python confirmed the imported module came from that environment. Browser verification against a temporary loopback-only server confirmed health, lower/upper UTC-overflow rejection for JSON/NDJSON, zero rejected events in search, valid UTC-normalized round-trip and rejection of unauthenticated search. The test server was shut down afterward.

Linux x86_64/Python 3.11.16 only was exercised, with temporary local SQLite files and no GPU. Isolated wheel installation is not clean-machine native installation acceptance. Arm64, Docker runtime, resource profiles, ClickHouse, production load, long retention and distributed scale remain unverified. Search-bound datetime validation was not changed or comprehensively verified in this slice.

## Source-publication blockers

- Add bounded query execution/cancellation and isolate synchronous SQLite work from the asynchronous event loop.
- Make local-evaluation Compose binding safe and verify real startup, volume ownership and persistence. Docker is unavailable in the current test environment.
- Establish a suitable private security-reporting process before an installable release.
- Obtain a fresh independent publication review after all blockers close, including adversarial timestamp-path review.

Next bounded daily slice: a SQLite query execution deadline with observed failing regressions before the fix. Only the interrupted ingestion-overflow slice was resumed; no additional milestone was started.

## Future capabilities

ClickHouse integration, browser UI, live tail, identity/RBAC, dashboards, alerts, retention, signed distribution and scale/failure validation are incomplete or unverified. See the roadmap for the intended sequence.

## Update cadence

After each normal daily development slice, publish a concise evidence-backed status/roadmap update when state changes. Weekly reports summarize verified progress and risks. Do not publish secrets, private logs, prompts, raw transcripts, unsupported claims or broken implementation to satisfy a cadence.
