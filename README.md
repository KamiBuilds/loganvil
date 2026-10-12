# LogAnvil

An original, open-source, self-hosted log analytics and troubleshooting platform under development.

**Status: public planning repository, not an installable release.** The implementation remains in its development workspace while publication-blocking correctness, security and deployment checks are closed. This repository currently contains the roadmap, architecture direction and verified progress summaries; it intentionally does not provide application code, a runnable installer, binaries or a production-readiness claim.

## Goals

- CPU-only operation without a GPU requirement.
- SQLite for lightweight development/homelab use; ClickHouse for the planned standard storage tier.
- Search, live tail, correlation, incident timelines, field extraction, saved searches, dashboards, alerts, runbooks and diagnostic bundles.
- Safe setup, least privilege, tenant isolation, identity/RBAC, audit evidence, retention/redaction, backup/restore, dependency review and reproducible releases.
- Linux x86_64 and arm64 first. Only Linux x86_64/Python 3.11.16 has been exercised in the current internal slice; arm64, containers and production scale are not verified.

## Milestone targets

- **Feature-complete beta:** 6 December 2026.
- **Security-hardened v1.0:** before 1 February 2027.

These are planning targets, not guarantees. Quality gates will not be weakened to meet a date. Development proceeds in bounded daily slices with weekly progress reports; this project is separate from the daily small-utility publishing program.

## Follow development

- [Roadmap](docs/ROADMAP.md)
- [Architecture direction](docs/ARCHITECTURE.md)
- [Current status and publication gates](docs/STATUS.md)
- [Contributing](CONTRIBUTING.md)
- [Security boundary](SECURITY.md)

Read the documentation locally with:

```sh
git clone https://github.com/KamiBuilds/loganvil.git
cd loganvil
```

There is **no application setup/run command yet in this public checkout**. Reviewed implementation and exact install/test instructions will be added after the source-publication gates pass. No GitHub Release is being made now.

## Scale and originality

The distributed design target includes 10,000,000+ registered device/source identities and months-to-years retention. This is an architectural objective, not a demonstrated capability or a single-node claim. Representative load, historical-query, soak, failure and recovery tests on documented hardware are required before making a support claim.

LogAnvil uses original APIs and a project-specific query language. It is not a certified or drop-in replacement for another product. The working name has not received professional trademark clearance.

## License

Apache License 2.0. See [LICENSE](LICENSE).
