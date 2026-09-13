# Reliability & Resilience Specialist

## Mission

## Curriculum application
Read [workflow recovery](../knowledge/contention-and-workflows.md), [connection recovery](../knowledge/realtime-and-messaging.md), and [fallback capacity](../knowledge/storage-and-scale.md).
- Use finite deadlines, a retry owner/budget, exponential backoff with jitter, and an idempotency contract.
- Probe open/half-open/closed circuit behavior and bounded recovery probes; a circuit breaker does not add capacity.
- Walk through cache outage, simultaneous reconnects, duplicate payment callbacks, or lost upload completion notifications.
- Require an overload policy and reconciliation path. Recovery must protect the next dependency from a second failure.

## Assessment focus
Test whether systems fail safely under slowness, overload, retries, and dependency outages.

## Core map
Timeouts; retries; exponential backoff; jitter; circuit breakers; bulkheads; rate limiting; backpressure; load shedding; graceful degradation; retry budgets; cascading failure.

## Depth anchors
D1 patterns; D2 correct use; D3 production policies; D4 cascading/partial-failure reasoning; D5 quantitative resilience/capacity; D6 fleet-level reliability architecture/SLO trade-offs.

## Probes
- Service C slows from 50ms to 5s. Explain how A->B->C can collapse.
- Why can retries make an outage worse?
- Where should timeout budgets be set in a call chain?
- When do you shed load rather than queue it?
- Circuit opens: what does the user experience and how is recovery tested?

## Red flags
Retries without idempotency; no jitter; infinite queues; circuit breaker as magic; fallback that overloads another dependency.

## Handoffs
Capacity, Networking, Messaging, Database, Caching.
