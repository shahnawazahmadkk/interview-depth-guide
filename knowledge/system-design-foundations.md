# System Design Foundations

Use this reference for mechanism teaching and design reviews. Select the section relevant to the current gap; do not lecture through the entire file.

## Networks and communication

Trace a request through DNS, connection establishment, TLS where applicable, a proxy/load balancer, application work, storage, and the return path. Account for cached DNS, reused connections, and time spent waiting for pools. An HTTP response does not inherently close its underlying connection.

| Choice | Appropriate requirement | Cost or limitation to explain |
|---|---|---|
| HTTP request/response | Ordinary resource reads and writes | Retry semantics, deadlines, payload size |
| TCP | Ordered reliable byte stream | Retransmission delay, connection state; delivery can still fail |
| UDP | Datagrams where the application can tolerate loss or supply recovery | No built-in delivery, ordering, flow control, or congestion control |
| QUIC / HTTP/3 | Modern encrypted transport with independent streams | QUIC runs over UDP; it is not a revision of TCP |
| gRPC | Typed internal RPC, streaming, efficient serialization | Client support, deadlines, operational tooling; browser clients need an adaptation layer |
| SSE | Server-to-client event stream | Proxy buffering, reconnect/replay, bounded client buffers |
| WebSocket | Frequent bidirectional messages | Connection ownership, reconnect, draining, application message protocol |
| WebRTC | Interactive audio/video or justified peer data exchange | Signaling, ICE, STUN discovery, TURN relay fallback; not always a direct path |

Do not claim TCP guarantees successful delivery despite permanent failures. It supplies an ordered byte stream or a connection failure, not application-level exactly-once execution. Match the transport to the clients and product requirements, rather than assuming every real-time workload uses UDP.

### Load balancing and geography

L4 routing uses transport-level information; L7 routing can inspect HTTP paths, hosts, headers, and cookies. Both can support WebSockets with appropriate implementations and configuration. Choose based on routing, TLS termination, connection limits, idle timeouts, and operational needs; WebSockets do not inherently require L4.

Distinguish routing a new connection from redistributing messages within an established connection. Long-lived connections need draining and client reconnection to move between servers. Least-connections can help, but message rate, fan-out, and CPU per connection may be uneven.

Client-side service discovery can remove a routing hop but requires membership refresh and failure handling. DNS caches and client behavior make DNS failover gradual, not instantaneous. Health checks should reflect ability to serve traffic without making a shared dependency outage restart the entire fleet.

For geographic latency, use a propagation lower bound of `2 * distance / propagation_speed` for a round trip. At approximately 200,000 km/s in fiber, 5,600 km implies about 56 ms before route length, queuing, or processing overhead. Co-locate compute and data; use regional partitioning when queries are naturally local, and edge caching when content can safely be reused.

## API contracts

Default to a small REST surface for product design. Consider GraphQL for flexible selection across diverse clients and gRPC for internal RPC when its benefits matter. Explain the choice briefly, then spend the interview on the dominant architectural problem.

Example resources: events, seats, reservations, payments.

```text
GET    /events?city=region-1&limit=20&cursor=opaque-token
GET    /events/{event_id}/seats
POST   /events/{event_id}/reservations
GET    /reservations/{reservation_id}
PATCH  /reservations/{reservation_id}
DELETE /reservations/{reservation_id}
```

Paths identify resources; query parameters filter, sort, or paginate; bodies carry structured mutation input. Plural resource nouns are a useful convention, not a protocol requirement. PUT replaces or creates at a known URI; PATCH expresses a partial change. GET must be safe; GET, PUT, and DELETE are defined to be idempotent, but the implementation must uphold that contract. POST and PATCH can also be made idempotent deliberately.

For pagination, define a stable total order, such as `(created_at, id)`, encode both values in the cursor, cap page size, and support an appropriate index. Cursor pagination reduces shifting-offset problems but does not itself create a consistent snapshot when sort keys change. Offset pagination supports page jumps but becomes costly at large offsets.

For retried mutations, scope an idempotency key to the caller and logical operation. Atomically claim the key with the mutation where possible, store a payload fingerprint and result, and handle concurrent duplicates and in-progress operations. Reject reuse with a different payload. Explain expiry and what happens after deduplication records expire. Propagate a stable operation identifier to external side effects.

Use consistent error envelopes with a machine-readable code and useful message. Distinguish authentication failures, authorization failures, conflicts, invalid input, rate limits, and transient server failures. A timeout leaves the outcome uncertain; an operation-status endpoint can resolve it. Include retry guidance without exposing secrets.

Evolve additively where possible. Removing or changing fields needs a compatibility and migration plan; URL versions are one option. Avoid inventing a new version for every additive field.

GraphQL requires schema/resolver authorization, query-cost limits, batching, and an approach to caching. N+1 queries can occur in REST or ORM code too; dynamic nested selection makes them easier to introduce. gRPC contracts require schema compatibility and deadline propagation; binary encoding is not a universal end-to-end speedup.

## Identity boundaries

Authenticate the caller and authorize each resource operation. A user identifier in a body is input, not proof of identity. HTTPS protects transport; it does not validate business permissions.

JWTs are typically signed, not encrypted. Validate allowed algorithms, issuer, audience, expiry, and signature; address revocation, key rotation, and stale permissions. Server-side sessions are also valid. API keys suit some application identities but are not the only internal authentication mechanism; scoped tokens and mTLS may fit better. Apply per-user, tenant, IP, and expensive-endpoint limits as appropriate. Permission and tenant filters must also cover caches, search results, vector retrieval, and blob downloads.

## Data modeling and indexes

Begin with entities, invariants, and concrete access patterns. For each important query identify predicates, ordering, result size, freshness, and write frequency. Choose a database by specific capabilities, rather than asserting that SQL cannot scale or NoSQL cannot model relationships.

Normalization reduces duplicated facts. Denormalization makes selected reads cheaper but introduces update propagation and reconciliation. Distinguish a historical snapshot, such as a price at purchase time, from a duplicated current value that must change everywhere.

B-tree indexes support equality and ranges. Composite indexes must match useful leading predicates and sort order. Covering indexes can reduce data-page access but cost storage and write work. Confirm with a query plan and representative data. Not every index helps, especially on low-selectivity predicates or write-heavy tables.

An inverted index maps terms to document references. Tokenization, normalization, ranking, and typo tolerance are separate decisions. Built-in full-text or geospatial indexes may suffice before adding a separate search service. CDC can update an external index asynchronously; plan for duplicates, lag, deletes, replay, and rebuilding.

## Caching, replication, and sharding

For cache-aside: look up the key, load from the authoritative store on a miss, populate with a TTL, and return. Specify the key, value structure, size, expiry, and invalidation policy. Write-through coordinates cache and durable writes; write-back acknowledges before durable persistence and has a loss window. Neither name alone proves consistency.

An invalidation after a write can still race with an older in-flight cache fill. Discuss versions, serialization, bounded staleness, or other safeguards according to the requirement. Single-flight reduces simultaneous regeneration of one key. TTL jitter spreads many-key expirations; it does not solve all contention on one hot key. A whole-cache outage requires admission control and a fallback whose downstream capacity is explicitly bounded.

Scale reads by improving queries/indexes, considering measured denormalization, adding replicas, and caching appropriate results. Replica lag affects read-your-writes and failover. Scale writes after identifying the actual bottleneck: batching, fewer unnecessary indexes, partitioning, or sharding may help.

Hash sharding distributes keys, not necessarily traffic. A popular account can still dominate a shard. Range sharding supports locality but can create hot ranges. Directory routing adds placement flexibility and a routing dependency. State the shard key and which queries become scatter-gather operations. Cross-shard transactions are possible but introduce coordination and failure costs.

Consistent hashing maps keys and nodes to a ring; virtual nodes improve balance. With uniform placement, adding one node to N nodes moves roughly `1/(N+1)` of keys. It does not eliminate skew, migration work, or failures. Redis Cluster instead uses 16,384 hash slots with explicit slot ownership and migration; do not describe it as a literal consistent-hash ring. See the [cluster specification](https://redis.io/docs/latest/operate/oss_and_stack/reference/cluster-spec/).

## Consistency

CAP concerns the inability to guarantee both linearizable consistency and availability during a partition. It is not a permanent menu of any two properties. Eventual consistency guarantees convergence under its assumptions, not an arbitrary freshness bound. PACELC also highlights coordination latency when the network is healthy.

Choose consistency per operation. A stale description may be acceptable while selling a specific seat requires a protected invariant. Read consistency alone does not prevent overselling; atomic writes, constraints, and transaction semantics matter. Ask what anomalies the product can tolerate and how it recovers.
