# Architecture direction

This page describes the design of an internal development slice and future direction, not application code available in this checkout.

## Current internal slice

```text
JSON/NDJSON HTTP boundary
  -> authentication, bounded input and event validation
  -> UUID / UTC / severity normalization
  -> EventStorage interface
  -> SQLite development/lightweight adapter

Project-specific Forge filters
  -> allowlisted clauses and parameterized SQL
  -> bounded time-window/result search
```

The internal slice uses Python 3.11, FastAPI and SQLite. It is CPU-only. Shared API-key authentication is an initial development mechanism, not identity/RBAC or tenant isolation. SQLite is a lightweight single-process design, not a horizontal-scale backend.

Existing request, attribute, time-window and result-count bounds do not yet provide query deadlines or event-loop isolation. Validated UTF-8 input also does not yet guarantee Unicode scalar validity in all nested values. These are publication blockers, not supported security properties; see [status](STATUS.md).

## Planned standard and distributed tiers

ClickHouse will implement the storage boundary for the standard tier, with tested migration, batch ingestion, retention and indexed query behavior. Distributed evolution will evaluate stateless ingestion, durable queues, explicit backpressure, tenant/source/time partitioning, replicas/shards, hot/warm/cold tiers, query pruning and bounded fan-out.

Indexing, caches, rollups and asynchronous historical queries must be justified by representative data shape and failure behavior. Hardware guidance remains planning-only until benchmarks measure events/s, bytes/s, active senders, registered identities, stored events/bytes, cardinality, selectivity, retention and p50/p95/p99 latency.

## Installation and operations

Native `uv` and container installation are planned public paths. Container startup/persistence and architecture support must be exercised before shipping. Guided setup, checksummed/signed artifacts, provenance/SBOM, restore drills and upgrade/rollback checks are release goals, not delivered capabilities.

No GPU is required by the roadmap. Linux x86_64 and arm64 are the first target platforms; current internal execution evidence covers Linux x86_64 only.

## Originality

Use original APIs, query syntax, UI, documentation and assets. No proprietary product code, branding or query-language imitation is part of this project. No certification or drop-in compatibility claim is made.
