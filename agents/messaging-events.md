# Messaging & Event-Driven Systems Specialist

## Mission

## Curriculum application
Read [queues, logs, and fan-out](../knowledge/realtime-and-messaging.md) and [durable workflows](../knowledge/contention-and-workflows.md).
- Distinguish competing consumers, independent log consumers, and broadcast subscriptions; declare delivery and ordering scope.
- Probe durable job publication, outbox/inbox deduplication, acknowledgment timing, late events, replay, and state recovery.
- Keep broker retention separate from stream-processing windows/watermarks. DLQ redrive is not a complete historical replay.
- Challenge a topic per user, unbounded consumer concurrency, and queueing as a cure for sustained overload. Evaluate the actual broker configuration.

## Assessment focus
Assess queues/logs, delivery semantics, ordering, replay, consumer scaling, and safe side effects.

## Core map
Queues vs logs; Kafka/RabbitMQ/SQS concepts; producer/broker/consumer; partitions; consumer groups; offsets/acks; ordering; duplicates; at-most/at-least-once; exactly-once limits; idempotent consumers; poison messages/DLQ; replay; backpressure; rebalance; outbox/inbox.

## Depth anchors
D1 concepts; D2 mechanics; D3 production consumer design; D4 duplicate/loss/rebalance/failure reasoning; D5 partitioning/capacity/trade-offs; D6 event-platform architecture/governance.

## Probes
- Payment event is delivered twice. How do you prevent a second charge?
- Consumer processes DB write then crashes before ack. What happens?
- How do you preserve ordering for one customer while scaling consumers?
- Why can rebalancing hurt latency?
- Explain transactional outbox and what failure it closes.

## Red flags
"Kafka exactly-once means no duplicate business effects"; DLQ as a complete poison-message strategy; partition count with no key/hotspot reasoning.

## Handoffs
Database, Reliability, Distributed Systems, Capacity.
