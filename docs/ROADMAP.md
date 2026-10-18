# Roadmap

Each phase includes tests, safe installation, documentation and independent review. Dates are targets, not permission to skip gates.

## 1. Complete the ingestion/search tracer slice

The internal SQLite/FastAPI slice has 132 passing tests on Linux x86_64/Python 3.11.16. Publication remains blocked; see [status](STATUS.md).

- Verified: reject invalid Unicode attribute keys/values before persistence, with sequential RED-GREEN regressions and valid Unicode round-trip checks.
- Verified: reject ingestion UTC normalization overflow before persistence, with eight RED-GREEN regressions and twelve valid-boundary compatibility controls. Search-bound datetime validation remains outside this slice's verified scope.
- Verified: cooperative SQLite search execution budget, including shared-connection/external database lock waiting, controlled expiration and cleanup/recovery. This is not a hard HTTP deadline or performance guarantee.
- Verified: synchronous ingestion persistence is awaited in the framework worker pool, with real-lock JSON/NDJSON scheduling regressions and rollback/recovery coverage. Parsing/normalization, worker saturation and overload control are not covered by a general responsiveness guarantee. Broader query budgets and explicit client-disconnect cancellation remain unfinished.
- Verified declaration only: local-evaluation Compose app publication is IPv4-loopback-only, with unpublished ClickHouse ports, no explicit networking overrides, a RED-GREEN mapping regression and proxy-boundary guidance. Docker runtime exposure, startup, volume ownership, persistence and restart remain unverified and blocked on an authorized disposable runtime.
- Re-run the full adversarial review before adding implementation to this public repository.

## 2. Standard storage tier

- Implement and test the ClickHouse storage adapter, migration/version strategy and health behavior.
- Establish native and container acceptance checks, with honest platform results.
- Measure ingest, query, storage and retention behavior rather than inferring capacity from CPU/RAM alone.

## 3. Search and troubleshooting

- Full-text/structured filters, stable pagination, cancellation, query budgets and explain diagnostics.
- Live tail with reconnect/backpressure, correlation IDs, incident timelines, field extraction, pattern comparison, runbook links and diagnostic bundles.

## 4. Browser experience

- Original search/event-detail UI, saved searches and field discovery.
- Keyboard accessibility, responsive layouts and meaningful empty/error/loading states.
- Safe setup-status visibility without secrets.

## 5. Dashboards, retention and recovery

- Query-backed panels and time controls, usage/disk-pressure safeguards and retention policy.
- Backup/restore with automated restore drills.
- Measured hot/warm/cold storage, indexing and rollup decisions.

## 6. Alerts and identity

- Scheduled alerts, deduplication, retry handling and delivery policies.
- Local identity, roles, tenant isolation, API-key lifecycle, rate limits and audit events.
- Threat models and abuse-case regressions.

## 7. Installation and release engineering

- Safe native and Compose workflows, guided setup and automation mode.
- Verify Linux x86_64 and arm64 before support claims.
- Dependency/license review, SBOM/provenance, signed artifacts, upgrade/rollback acceptance and operational runbooks.

## 8. Release acceptance

- Feature-complete beta target: **6 December 2026**.
- Security-hardened v1.0 target: **before 1 February 2027**.
- Complete security, load, soak, failure, recovery, upgrade and restore tests with independent approval.
- Publish reproducible hardware/results matrices, known limits and exact installation/test instructions.

## Longer-term distributed architecture

Stateless ingestion, durable buffering, backpressure, deterministic partitioning, ClickHouse sharding/replication, tiered object storage, bounded query fan-out and recovery/rebalance require representative validation. The 10,000,000+ registered-identity and months-to-years-history goal is unverified and is not an MVP/single-node capability claim.

SSO/SAML, advanced anomaly detection, a managed service and non-Linux production support are not current commitments.
