# Development status

Updated: 2026-10-20 (recovery of the 19 October daily slice).

## Public distribution

This remains a documentation/planning repository. No application source, installable package, executable container or GitHub Release is distributed here. Local tests and scoped review do not replace the full source-publication gate.

## Verified internal progress

The internal SQLite/FastAPI tracer slice provides bounded JSON/NDJSON ingestion, API-key checks, basic time-windowed search, restrictive SQLite paths, a time-ordering index, cooperative search execution budgets and awaited worker-pool ingestion persistence. Earlier validation rejects invalid Unicode attributes and ingestion timestamps that cannot be represented in UTC. The checked-in Compose application mapping is IPv4-loopback-only; actual container exposure and persistence remain unverified.

The recovered bounded slice rejects search bounds that overflow during UTC normalization with a fixed JSON `422`, before range arithmetic or storage. Eight lower/upper-bound regressions failed with `500` before the fix and now pass, including extreme offsets and microsecond-adjacent overflow. Thirteen additional first-run-passing compatibility cases cover representable UTC boundaries, real ingestion/search round-trips, ordering and the 31-day window limit; they are not additional historical RED-GREEN cycles.

Search remains inclusive at `start` and exclusive at `end`. The exact maximum UTC instant can be stored but cannot be included in a search: there is no larger representable exclusive end. This limitation was documented, not changed.

Verification refreshed during recovery:

- 25 focused search tests and 153 tests overall passed, with one unchanged upstream Starlette test-client deprecation warning; none suppressed.
- Ruff lint/format, strict mypy for 11 source files, compilation, frozen-lock validation, wheel/source-distribution build and diff checks passed.
- Frozen exported runtime/development dependency audit found no known vulnerabilities. Added-line security scan found no flagged patterns; secret scan retained six reviewed documentation/test-fixture findings, none new.
- Fresh independent scoped security/correctness review passed with no findings, including full/focused suite and static-check reruns. Parent gates were rerun after review. This is not publication or deployment approval.
- The rebuilt wheel was installed with frozen dependencies into a disposable environment. Isolated Python outside the source checkout verified installed module paths and passed all 25 search tests, with only the same upstream warning.
- Real-browser requests against the installed wheel on an ephemeral loopback-only server verified health `200`, four fixed invalid-bound `422` responses, three boundary ingests `202`, correct low/high offset-window search results `200`, and unauthenticated search `401`. The owned server was stopped afterward and verified unreachable.

Linux x86_64/Python 3.11.16 was exercised with temporary SQLite and no GPU. An isolated wheel install is not clean-machine native installation acceptance. Docker remains unavailable. Compose rendering/build/startup, effective network exposure, volume ownership/persistence/restart, arm64, ClickHouse, resource profiles, load/soak, historical retention and distributed recovery remain **unverified**. No scale benchmark was run.

## Limits and source-publication blockers

Ingestion persistence isolation is not whole-request isolation: parsing/validation/normalization still run on the event loop, and shared worker saturation can delay health and other requests. No ingestion deadline or overload admission control exists. Search limits remain cooperative, not a hard HTTP wall-clock deadline. A loopback declaration does not isolate local callers or prove effective Docker/firewall configuration; ordinary local processes and same-network containers remain within the trust boundary. Operator guidance requires a maintained Docker Engine >=28.0.0 because of the older same-L2 localhost-publishing bypass, but the version floor alone does not validate exposure.

Publication remains held for:

- Broader cancellation/query-budget acceptance, including explicit client-disconnect cancellation. The scoped search-bound UTC-overflow check is now closed, not the full query/security gate.
- Actual Compose startup, effective exposure, volume ownership, persistence and restart verification on an authorized disposable runtime. Docker is unavailable in the current environment.
- A suitable private security-reporting process before an installable release.
- Fresh full independent publication review after the blockers close.

Week 1 remains in progress. This recovery completes only the interrupted daily slice; no additional milestone was started. Next scheduled bounded slice: explicit search client-disconnect cancellation, beginning with a failing cancellation/connection-recovery regression. Container acceptance stays separately blocked.

## Future capabilities

ClickHouse integration, browser UI, live tail, identity/RBAC, dashboards, alerts, retention, signed distribution and scale/failure validation are incomplete or unverified. See the roadmap for the intended sequence. The 10,000,000+ registered-identity goal remains an unverified distributed architectural target, not a current support claim.

## Update cadence

After each normal daily development slice, publish concise evidence-backed status/roadmap changes when verified state changes. Weekly reports summarize progress and risks. Never publish secrets, private logs, prompts, raw transcripts, unsupported claims or broken implementation to satisfy a cadence.
