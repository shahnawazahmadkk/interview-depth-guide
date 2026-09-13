# Canonical Engineering Taxonomy

The 15 specialist domains are the top-level taxonomy. Each concept should be represented as:

- `id`
- `name`
- `domain`
- `prerequisites[]`
- `related[]`
- `depth.D0..D6`
- `failure_modes[]`
- `interview_signals[]`
- `evidence_refs[]`

Depth is not question difficulty. It is the demonstrated reasoning depth on a concept.

## Cross-domain examples

`cache_stampede` -> caching, reliability, database, capacity, distributed-systems

`refresh_token_rotation` -> identity, database/session-storage, distributed-systems, API

`payment_idempotency` -> API, database, messaging, reliability, distributed-systems

`connection_pool_exhaustion` -> java-runtime, database, networking, capacity, reliability
