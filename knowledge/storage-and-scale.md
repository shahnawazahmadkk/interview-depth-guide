# Storage and Quantitative Scaling

## Large objects

Store searchable metadata and permissions in the authoritative database. Store large media/files in object storage when that fits the access model. The database stores an object identifier and lifecycle state, rather than an expiring signed URL as the durable identity.

Upload path:

1. Authorize the upload, enforce size/type/quota limits, and create pending metadata.
2. Issue a short-lived credential scoped to the intended object and operation.
3. Client uploads directly, using multipart/resumable transfer for large files.
4. Verify completion and integrity before marking metadata ready.
5. Trigger any scanning/transcoding/indexing through reliable background processing.

Completion notifications can be duplicated or lost. Make handlers idempotent and reconcile pending records against storage. Expire abandoned multipart uploads and remove or quarantine orphaned objects. Define who may finalize or delete an upload and protect against overwriting another user's object.

Download path: authorize access, provide the appropriate scoped object or CDN credential, and serve through an edge cache when useful. A storage presigned URL and a CDN signed URL are not interchangeable. Define cache keys, authorization boundaries, expiry, invalidation, range requests, and origin protection. Private data must not leak through shared cache keys.

Managed storage is scalable but not literally unlimited: account quotas, request rates, partition behavior, bandwidth, and cost still apply. Verify current vendor limits or prices only when the decision requires them.

## Scale only the identified bottleneck

For reads, improve query/access patterns, indexing, projection sizes, and avoidable joins; then consider replicas and caches. For writes, reduce unnecessary work, batch when latency allows, distribute independent data, and protect hot keys. Vertical scaling may be simpler than introducing shards. Horizontal scaling requires a routing and state strategy.

Regional partitioning fits geographically local workloads; geospatial indexing serves proximity queries within a region. Small candidate sets may be scanned directly. For larger sets, choose an appropriate geospatial index and discuss region boundaries, moving objects, and stale coordinates.

## Calculation toolkit

All performance numbers are assumptions until measured for a workload and configuration. Do not encode a fixed TPS, connection count, dataset size, or CPU percentage as a universal sharding trigger.

| Decision | Calculation | Caveat |
|---|---|---|
| Average traffic | events per day / 86,400 | Apply a peak factor and geographic skew |
| Outstanding requests | arrival rate * mean residence time | Stable averages; queued work and sockets are different populations |
| Polling load | connected clients / interval seconds | Include retries and background tabs |
| Cache fallback | read QPS * (1 - hit fraction) | At outage, hit fraction may become zero |
| Bandwidth | operations/s * bytes/operation | Add replication, fan-out, protocol overhead |
| Storage | items/s * bytes/item * retention seconds | Add indexes, replicas, metadata, compaction space |
| Worker count | arrival rate * mean task duration / utilization target | Bound by downstream capacity and ordering constraints |
| Queue growth | arrival rate - service rate | Applies while arrivals exceed completion rate |
| Backlog drain | backlog / (service rate - ongoing arrivals) | Finite only with positive spare service rate |
| Vector bytes | vectors * dimensions * bytes/dimension | Index overhead and working memory are additional |

For interview assumptions, memory accesses are much faster than local storage, which is generally faster than remote work. Actual end-to-end latency includes queueing and software. Ask for or state a plausible latency budget instead of memorizing an unconditional cache/database speedup.

Worked checks:

- 50,000 reads/s against an 8,000 QPS database requires a hit fraction of at least 84% before headroom or write costs. A full cache outage overwhelms this database.
- At 200 KB per response and 10,000 responses/s, payload egress is 2 GB/s, approximately 16 Gbit/s using decimal units.
- A backlog of 6,000 tasks drains in 60 seconds with 300 completions/s and 200 new tasks/s.
- Ten million sockets at 10,000 per server require 1,000 servers before redundancy, not 100.
- One million 1,536-dimensional float32 vectors occupy 6.144 GB of raw vector data, excluding identifiers and indexes.

## Evidence for scaling

Use sustained p95/p99 latency against the objective, queue age, saturation, cache churn, replication lag, partition skew, and growth forecasts together. High CPU or memory can be a signal but not a diagnosis. Low CPU can coexist with exhausted connection pools, lock waits, or storage bottlenecks.

Before adding capacity, identify what is saturated, test a representative workload, include failure headroom, and estimate migration cost. Explicitly state what happens when one node, zone, or region is unavailable. Autoscaling needs a useful metric, a provisioning delay, and overload handling during that delay.
