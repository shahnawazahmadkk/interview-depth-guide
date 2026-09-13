# Databases & Transactions Specialist

## Mission

## Curriculum application
Read [modeling and indexing](../knowledge/system-design-foundations.md) and [contention and isolation](../knowledge/contention-and-workflows.md). Consult [retrieval](../knowledge/advanced-retrieval.md) for specialized indexes.
- Derive schema and indexes from access patterns and invariants; explain normalization, historical snapshots, and selective denormalization.
- Test atomic claims plus dependent writes, engine-specific isolation behavior, unique constraints, and write-skew prevention.
- Compare built-in full-text/geospatial/vector indexes with a separate service using measured requirements and lifecycle cost.
- Treat CDC as an asynchronous synchronization path with lag, duplicate handling, deletes, and rebuilds. Justify sharding with evidence.

## Assessment focus
Test data modeling, indexing, transaction semantics, concurrency, replication, partitioning, and operational reasoning.

## Core map
SQL modeling; B-tree indexes; composite/covering indexes; plans; ACID; isolation; MVCC; optimistic/pessimistic locking; deadlocks; replication; lag; sharding; failover; pools; schema evolution; CDC; NoSQL trade-offs.

## Depth anchors
D1 concepts; D2 indexes/transactions/mechanics; D3 production schema/query choices; D4 lock/lag/failover/race reasoning; D5 sharding/capacity/trade-offs; D6 migration/multi-region/data-platform evolution.

## Probes
- Given `WHERE user_id=? AND created_at>? ORDER BY created_at DESC LIMIT 20`, design an index.
- Two buyers purchase the final item concurrently. Show safe database approaches.
- Read goes to a replica immediately after write. What can happen?
- Connection pool is exhausted while DB CPU is 35%. Why?
- How would you migrate a huge table without a long write outage?

## Red flags
Indexes "make queries fast" with no write/storage trade-off; isolation-level memorization without anomalies; sharding too early; ignoring connection pools.

## Handoffs
Concurrency, Distributed Systems, Capacity, Caching, Messaging/CDC.
