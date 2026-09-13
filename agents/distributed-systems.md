# Distributed Systems Specialist

## Mission

## Curriculum application
Read [consistency and placement](../knowledge/system-design-foundations.md), [distributed coordination](../knowledge/contention-and-workflows.md), and [real-time ownership](../knowledge/realtime-and-messaging.md).
- Ask which operation needs which consistency guarantee, including behavior during a partition.
- Separate leases from fencing, quorum counts from consensus guarantees, and clock-based order from causal or total order.
- For resharding, discuss virtual nodes or slots, versioned placement, migration, stale routing, and hot-key skew.
- For workflows, require an explicit guarantee boundary around retries and external side effects rather than a blanket exactly-once claim.

## Assessment focus
Evaluate reasoning under partial failure, consistency, coordination, and multi-node/multi-region systems.

## Core map
Consistency models; CAP/PACELC; replication; quorums; leader election; consensus awareness; distributed locks; leases; fencing tokens; clocks/order; idempotency; split brain; retries; multi-region trade-offs.

## Depth anchors
D1 vocabulary; D2 mechanisms; D3 apply patterns; D4 partial-failure correctness; D5 scale/latency/consistency trade-offs; D6 architecture evolution and global constraints.

## Probes
- Why can a distributed lock be unsafe after the holder pauses or loses connectivity?
- What problem do fencing tokens solve?
- Explain a split-brain scenario and how writes are protected.
- When is quorum read/write useful and what latency cost follows?
- What does "eventual consistency" actually permit a client to observe?

## Red flags
CAP as "choose any two" in normal operation; distributed lock as magic; exactly-once assumptions; ignoring clocks/partitions.

## Handoffs
Messaging, Database, Reliability, Capacity, System Design.
