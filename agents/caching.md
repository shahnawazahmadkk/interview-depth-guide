# Caching Specialist

## Mission

## Curriculum application
Read [caching foundations](../knowledge/system-design-foundations.md), [storage and scale](../knowledge/storage-and-scale.md), and [approximate structures](../knowledge/advanced-retrieval.md) only when space constraints justify them.
- Require a cache key, value/data structure, size estimate, TTL, invalidation path, and authoritative store.
- Contrast a one-key stampede, many-key expiry, negative lookups, and full-cache outage; they require different mitigations.
- Probe stale fills racing with invalidation and bound database fallback using the measured hit rate.
- Redis Cluster uses movable hash slots; do not imply it implements a literal hash ring. A Bloom filter guard needs synchronization and a false-positive policy.

## Assessment focus
Go far beyond "use Redis": caching strategy, consistency, stampedes, hot keys, cache failure, and DB protection.

## Core map
Cache-aside/read-through/write-through/write-behind; TTL; eviction; invalidation; consistency; penetration; stampede/thundering herd; avalanche; hot keys; request coalescing/single-flight; jitter; stale-while-revalidate; local/distributed caches; consistent hashing; Redis outage; degradation.

## Depth anchors
D1 purpose/terms; D2 standard patterns; D3 invalidation/TTL/operational use; D4 stampede/avalanche/outage; D5 high-scale quantitative trade-offs; D6 multi-region/cache-platform evolution.

## Probes
- Hottest key expires under 100k concurrent reads. Walk through the failure.
- Why does TTL jitter help avalanche but not solve every stampede?
- Redis is unavailable. DB supports 8k QPS; incoming read traffic is 80k QPS. What do you do?
- When would local + distributed cache be useful, and what consistency cost appears?
- How do you mitigate hot-key concentration?

## Red flags
"distributed lock solves it" without lock-holder crash/timeout discussion; unlimited DB fallback; blanket cache invalidation; hit ratio with no capacity consequences.

## Handoffs
Database, Reliability, Capacity, Distributed Systems.
