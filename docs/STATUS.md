# Development status

Updated: 2026-10-18.

## Public distribution

This remains a documentation/planning repository. No application source, installable package, executable container or GitHub Release is distributed here. Local tests and a scoped review do not replace the full source-publication gate.

## Verified internal progress

The internal SQLite/FastAPI tracer slice provides bounded JSON/NDJSON ingestion, API-key checks, basic time-windowed search, restrictive SQLite paths, a time-ordering index, cooperative search execution budgets and awaited worker-pool ingestion persistence. Earlier validation rejects invalid Unicode attributes and ingestion timestamps that cannot be represented in UTC.

The latest bounded slice changes the checked-in Compose application mapping from all-interface publication to IPv4 loopback only (`127.0.0.1:8000:8000`). Its exact-mapping regression failed against the original declaration and passed after the one-line fix. An additional first-run-passing compatibility control verifies that ClickHouse remains unpublished and no explicit networking overrides are declared; it is not an additional historical RED-GREEN claim.

Internal operator guidance now distinguishes host and proxy-container loopback, preserves API-key authentication, and explains TLS/access/rate/body/timeout requirements, log/header risks and remaining trust boundaries. Ordinary local processes and same-network containers can still reach services. Guidance requires a maintained Docker Engine >=28.0.0 because Docker documents an older same-L2 localhost-publishing bypass; that version floor alone does not validate network isolation. Future disposable-host acceptance must test host LAN/IPv6 and container-IP exposure, rendered overrides, ownership, persistence and restart behavior.

Verification passed:

- Five focused distribution tests; 132 automated tests overall, with one unchanged upstream Starlette test-client deprecation warning. No warnings suppressed.
- Ruff lint/format, strict mypy for 11 source files, compilation, frozen-lock validation, wheel/source-distribution build and diff checks.
- Frozen exported dependency audit: no known vulnerabilities. This does not audit the unpublished application package through PyPI.
- Added-line security scan: no flagged patterns. Tracked-file secret scan: the same six reviewed documentation/test-fixture findings, none introduced. An additional all-files scan flagged the standard generated pytest cache marker, not a secret or publication input.
- Fresh independent scoped security/correctness review, including full-suite/static-check reruns. This is not source-publication or deployment approval.

Linux x86_64/Python 3.11.16 was exercised with temporary SQLite and no GPU. No application/storage source changed in this slice. Docker is still unavailable: Compose rendering, container build/startup, actual loopback/remote-access enforcement, volume ownership/persistence/restart and proxy deployment were **not verified**. A parsed YAML regression and Python package build are not container acceptance. No new clean-machine native installation, arm64, resource-profile, ClickHouse, load, retention or distributed-scale evidence was produced.

## Limits and source-publication blockers

Existing ingestion persistence isolation is not whole-request isolation: parsing/validation/normalization still run on the event loop, and shared worker saturation can delay health and other requests. No ingestion deadline or overload admission control exists. Search limits remain cooperative, not a hard HTTP wall-clock deadline. A loopback declaration does not isolate local callers or prove the effective Docker/firewall configuration.

Publication remains held for:

- Broader cancellation/query-budget acceptance, including explicit client-disconnect cancellation and adversarial search-bound datetime review.
- Actual Compose startup, effective exposure, volume ownership, persistence and restart verification on an authorized disposable runtime. Docker is unavailable in the current environment.
- A suitable private security-reporting process before an installable release.
- Fresh full independent publication review after the blockers close.

Week 1 remains in progress. Recovery of this daily slice is complete; no additional milestone was started. Next bounded daily slice: adversarial search-bound UTC range validation review and controlled API boundary tests, with failing-before-fix evidence for any repair. Container acceptance stays separately blocked.

## Future capabilities

ClickHouse integration, browser UI, live tail, identity/RBAC, dashboards, alerts, retention, signed distribution and scale/failure validation are incomplete or unverified. See the roadmap for the intended sequence. The 10,000,000+ registered-identity goal remains an unverified distributed architectural target, not a current support claim.

## Update cadence

After each normal daily development slice, publish concise evidence-backed status/roadmap changes when verified state changes. Weekly reports summarize progress and risks. Never publish secrets, private logs, prompts, raw transcripts, unsupported claims or broken implementation to satisfy a cadence.
