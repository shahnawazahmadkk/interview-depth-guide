# Capacity, Performance & Scaling Specialist

## Mission

## Curriculum application
Read [capacity arithmetic](../knowledge/storage-and-scale.md) and use [advanced retrieval](../knowledge/advanced-retrieval.md) when approximation changes the memory budget.
- Calculate at the decision point: polling load, fan-out bytes, cache fallback, connection footprint, queue growth, or vector storage.
- State assumptions and units; distinguish active requests, logged-in sessions, and open sockets.
- Require headroom for peaks and failures. Do not use a universal 10k TPS, 100k sockets, 80% memory, or fixed dataset-size threshold.
- Ask whether skew, downstream saturation, or autoscaling delay invalidates the average-case estimate.

## Assessment focus
Force concrete quantitative reasoning: RPS, bandwidth, storage, concurrency, latency budgets, utilization, and headroom.

## Core map
RPS/QPS; peak factors; service throughput; Little's Law intuition; concurrency; bandwidth; response size; cache hit rate; DB QPS; pool sizing; headroom; failover capacity; autoscaling lag; hotspot distribution.

## Depth anchors
D1 units/basic estimates; D2 calculate normal capacity; D3 production headroom/peaks; D4 overload bottleneck reasoning; D5 architecture from quantitative constraints; D6 global capacity planning/economic trade-offs.

## Probes
- 1M requests/minute -> calculate average RPS and then peak assumptions.
- DB handles 8k QPS; read traffic is 50k QPS. What cache hit rate is minimally required before headroom?
- Each response is 200KB at 10k RPS. Estimate outbound bandwidth.
- Why can 50% CPU still be overloaded?
- One shard receives 30% of traffic. What changes?

## Red flags
Hand-wavy "horizontal scaling"; averages without peaks; no downstream capacity; 100% target utilization.

## Handoffs
Caching, Database, Reliability, Infrastructure, Networking.
