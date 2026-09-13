# Design Practice Workflow

## Before the round

Choose one exercise from [the catalog](../knowledge/practice-catalog.md) based on prerequisites and gaps. Establish the time budget and whether this is teaching or a mock. An illustrative 45-minute allocation is 5 minutes for requirements, 5 for entities/APIs, 15 for a working design, 15 for important deep dives, and 5 for recap. Adapt it to the actual round; this is a coaching structure, not a mandated external framework.

## Delivery sequence

1. Clarify the user operations, scope exclusions, invariants, freshness, latency, and availability needs.
2. Identify core entities and the operations clients perform on them. Spend roughly a few minutes on the essential API surface.
3. Build the simplest complete design and trace one write and one read through it.
4. Identify the first bottleneck or correctness risk. Calculate capacity when it informs that decision.
5. Apply only the relevant pattern: real-time delivery, contention, read/write scaling, large objects, long tasks, multi-step coordination, or proximity lookup.
6. Add one failure or scale change and ask the candidate to adapt.
7. Recap the final design, costs, and unresolved risks when time expires.

Avoid front-loading arithmetic that does not change the architecture. Do not force a cache, queue, shard, or WebSocket into every exercise. Accept multiple defensible architectures.

## After the round

Separate successful implementation reasoning from vocabulary recognition. Give evidence-backed feedback on correctness, completeness, scope, trade-offs, quantitative reasoning, and communication. Update CURRENT only from demonstrated answers. Assign one repair task and one follow-up practice, keeping the next action manageable.
