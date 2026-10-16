# Development status

Updated: 2026-10-16.

## Public distribution

This is a documentation/planning repository. No application code, installable package, executable container or GitHub Release is currently distributed here. Public source publication remains withheld; passing local checks and a scoped fix review do not replace the full publication gate.

## Verified internal progress

The internal SQLite/FastAPI tracer slice provides bounded JSON/NDJSON ingestion, shared API-key checks, basic time-windowed search, restrictive SQLite paths and a time-ordering index. Earlier work rejects invalid Unicode attributes and ingestion timestamps that cannot be represented in UTC.

The latest bounded slice adds a server-controlled one-second **cooperative SQLite search execution budget**. Shared-connection lock waiting and external SQLite busy waiting consume the same budget. A progress handler interrupts expensive SQL, and short queries check expiration after fetching. Expiration returns a fixed `504` without partial results; cleanup restores the previous busy timeout and clears the handler before releasing the connection lock.

Three sequential RED-GREEN cycles covered the API response, a real held adapter lock and a real external SQLite exclusive lock. Additional controls verify actual SQLite interruption, short-query expiration and preservation of unrelated schema errors/custom timeouts. Subsequent writes and searches recover.

Verification passed:

- 127 automated tests, with one unchanged upstream Starlette test-client deprecation warning.
- Ruff lint/format and strict mypy checks for 11 source files.
- Compile checks, frozen dependency-lock validation and wheel/source-distribution build.
- Exported frozen dependency audit: no known vulnerabilities. This does not audit the unpublished application package through PyPI.
- Added-line security scan: two formatted-SQL flags reviewed as internal integer-only timeout pragmas, not user-value interpolation. The tracked secret scan retained six reviewed documentation/test-fixture findings, with no new findings.
- Independent security/correctness review of this deadline slice, including an independent full-suite/static-check rerun. This is not approval for source publication.

An isolated uv environment installed the built wheel with frozen runtime dependencies. Outside-source isolated Python confirmed the imported module came from that environment. Real-browser checks against a temporary loopback-only server verified health, authenticated ingest/search and rejection of unauthenticated search. A real external database lock caused the controlled `504`; releasing it restored ingest/search with both stored events preserved. The owned test server was stopped and verified unreachable afterward.

Linux x86_64/Python 3.11.16 only was exercised, with temporary local SQLite files and no GPU. Isolated wheel installation is not clean-machine native installation acceptance. Arm64, Docker runtime, resource profiles, ClickHouse, production load, long retention and distributed scale remain unverified.

The cooperative budget is **not a hard end-to-end HTTP deadline or performance guarantee**. OS I/O, individual SQLite instructions, result decoding/validation, response serialization and scheduling can exceed it. Explicit client-disconnect cancellation remains unfinished. Search-bound datetime validation was not changed or comprehensively verified in this slice.

## Source-publication blockers

- Isolate synchronous ingestion SQLite persistence from the asynchronous event loop; broader cancellation/query-budget acceptance remains open.
- Make local-evaluation Compose binding safe and verify real startup, volume ownership and persistence. Docker is unavailable in the current test environment.
- Establish a suitable private security-reporting process before an installable release.
- Obtain a fresh independent publication review after all blockers close, including adversarial timestamp/query-path review.

Next bounded daily slice: isolate synchronous ingestion persistence from the event loop, with an observed failing real-lock scheduling regression before the fix. No additional milestone was started in this run.

## Future capabilities

ClickHouse integration, browser UI, live tail, identity/RBAC, dashboards, alerts, retention, signed distribution and scale/failure validation are incomplete or unverified. See the roadmap for the intended sequence.

## Update cadence

After each normal daily development slice, publish a concise evidence-backed status/roadmap update when state changes. Weekly reports summarize verified progress and risks. Do not publish secrets, private logs, prompts, raw transcripts, unsupported claims or broken implementation to satisfy a cadence.
