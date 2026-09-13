# Contention and Durable Work

## Protect the actual invariant

Begin by stating the invariant: inventory never becomes negative, one active owner per seat, or at least one eligible worker remains on duty. Identify the authoritative state and all writers. A local mutex cannot coordinate independent service instances.

Differentiate failures: overwriting a newer value with a stale computed value is a lost update; two unconditional atomic decrements can instead produce a negative counter without losing either update. Both can violate the business invariant.

### Conditional writes

Use an atomic predicate when the invariant can be checked at write time:

```sql
UPDATE inventory
SET available = available - 1
WHERE item_id = :item_id AND available > 0
RETURNING item_id;
```

Check the affected-row count. Zero rows is often a normal conflict outcome, not an exception. Dependent writes must not execute when this claim fails. Place a claim and its reservation record in one transaction, using the returned rows or explicit rollback. External charging is not made atomic by a local database transaction.

A quantity guard protects a quantity, not a specific seat. For assigned seats, use a unique `(event_id, seat_id)` row and a conditional transition from available to reserved. Enforce uniqueness in the schema. For a group booking, all required claims must succeed in one transaction or roll back.

### Pessimistic and optimistic approaches

Pessimistic locking holds the relevant rows while application code makes a decision. Keep scope and duration small, use a consistent acquisition order, and keep remote I/O outside the lock. Ordinary snapshot reads need not be blocked by a row lock. Database behavior depends on engine and isolation level; consult [PostgreSQL locking semantics](https://www.postgresql.org/docs/current/explicit-locking.html).

Optimistic concurrency reads a version and writes only if the version still matches. Every participating writer must update the version. Check the affected-row count and bound retries. Protect the business predicate as well as the version. A monotonic version avoids the ABA problem, where a business value changes away and back before comparison.

For selecting adjacent seats, either lock before selecting or select optimistically and conditionally claim all chosen rows in a transaction. The latter must check that every seat was claimed and roll back otherwise. Application-side selection does not rule out conditional writes.

Choose based on contention and wasted retry work. OCC is not lock-free at the database engine level; it avoids holding an explicit lock across the application decision interval.

### Isolation and write skew

If two workers each observe the other on duty and independently remove their own row from duty, both may commit under snapshot isolation. They update different rows, so per-row version checks alone miss the invariant.

Solutions include serializable transactions with whole-transaction retries, locking a shared guard row, or appropriately locking the complete invariant set. Phantom rows and inserts can complicate a rows-only lock strategy. Explain the actual isolation guarantees of the chosen engine, not just the level's name. The supplied learning material stops partway through this topic; use this as independent background, not as a reconstruction of missing text.

## Reservations and distributed coordination

Represent a long checkout hold as durable reservation state with an expiry and owner, rather than a database transaction held open while a person pays. A sweeper can release expired holds, but finalization must itself atomically verify ownership and expiry. Use an authoritative time policy and resolve payment success arriving after expiry.

Distributed leases expire even if a paused holder resumes later. Check owner tokens on release and use fencing or authoritative conditional writes to reject stale holders. A majority of independent caches is not interchangeable with a consensus protocol. Choose correctness guarantees according to the resource, not the popularity of a locking algorithm.

Queue serialization can reduce competing writers if every mutation for an entity reaches one ordered lane. Still handle duplicates, failover, rebalancing, and side effects outside that lane. A global lane trades away throughput and availability.

## Long-running tasks

Use a synchronous response when the operation is short and its outcome fits the latency budget. Otherwise:

1. Validate and authorize the request; establish a stable operation identifier.
2. Durably record the job and publish work reliably, for example through an outbox.
3. Return an acknowledgment and job-status resource.
4. Workers claim work, report progress, and commit durable results before acknowledging.
5. Clients poll, long-poll, or subscribe for status according to latency needs.

Track pending, running, succeeded, failed, and cancelled states with permitted transitions. Distinguish requesting cancellation from successfully stopping an external effect. Bound worker concurrency, queue age, attempts, and execution deadlines. Support poison-message isolation, replay controls, and reconciliation for jobs stranded by failures.

At-least-once execution requires idempotent results and side effects. A worker crash after a write but before acknowledgment must not duplicate business effects. A lease/visibility timeout is not proof that the old worker stopped.

## Multi-step processes

For order fulfillment, coordinate reservation, payment, fulfillment, and notifications with durable state. A simple state machine may suffice; a workflow engine becomes useful when retries, timers, recovery, and visibility dominate custom code.

Orchestration centralizes sequencing. Event choreography decouples services but can hide the process state and complicate recovery. An event log alone does not constitute a complete workflow engine or an event-sourced application.

External activities may execute more than once even with durable workflow replay. Do not promise exactly-once business effects without a specific end-to-end guarantee. Give each activity an idempotency contract, timeout, retry policy, and reconciliation path. Compensation is a business action, such as releasing a reservation or issuing a refund; it may itself fail and is not a perfect database rollback.

## Reliability drill

Inject one failure: timeout after successful charging, duplicate event, expired hold, reordered callback, poisoned job, or coordinator restart. Ask the candidate to identify the durable state, who owns the retry, how duplication is prevented, and how an operator verifies recovery.
