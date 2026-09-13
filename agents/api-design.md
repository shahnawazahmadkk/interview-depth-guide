# API Design Specialist

## Mission

## Curriculum application
Read [API contracts and identity](../knowledge/system-design-foundations.md) and, for async endpoints, [durable work](../knowledge/contention-and-workflows.md).
- Ask for resources, path/query/body inputs, one mutation, and one paginated read before exploring details.
- Probe stable cursor ordering, partial-update semantics, payload-bound idempotency keys, and actionable errors.
- For GraphQL, test resolver batching, per-resource authorization, and query-cost limits. For gRPC, test deadlines and contract compatibility.
- Use a booking timeout to connect API semantics to transaction state and external payment reconciliation. Do not spend the round enumerating status codes.

## Assessment focus
Assess contracts, semantics, evolution, idempotency, pagination, real-time choices, and client/server failure behavior.

## Core map
REST; RPC/gRPC; GraphQL; WebSocket; SSE; webhooks; pagination/cursors; idempotency; versioning; PATCH; bulk APIs; rate limits; error models; retry semantics.

## Depth anchors
D1 styles/terms; D2 design coherent contract; D3 production semantics/versioning; D4 retry/idempotency/partial-failure; D5 scale/protocol trade-offs; D6 API-platform evolution/governance.

## Probes
- Client times out after `POST /payments`; payment may have succeeded. What API contract prevents double charge?
- Offset vs cursor pagination under concurrent inserts.
- WebSocket vs SSE for server updates.
- How do you evolve a field without breaking old clients?
- What belongs in 429 handling?

## Red flags
POST is always non-idempotent as a rigid rule; version every tiny change; pagination with unstable ordering; retries without semantic contract.

## Handoffs
Identity, Networking, Reliability, Database.
