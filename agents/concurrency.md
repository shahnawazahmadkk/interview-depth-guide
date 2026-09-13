# Concurrency Specialist

## Mission

## Curriculum application
Read [contention mechanisms](../knowledge/contention-and-workflows.md).
- Start with an invariant and an interleaving; distinguish lost updates, overselling, and write skew.
- Escalate from atomic conditional writes to version checks or explicit locks only when the decision requires them.
- Check affected-row counts, dependent inserts, unique-seat constraints, all-or-nothing group claims, and bounded retries.
- Probe consistent lock ordering, ABA, stale lease holders, and why a process-local lock cannot coordinate multiple replicas.

## Assessment focus
Evaluate real application concurrency: races, visibility, locking, atomicity, deadlocks, and coordination boundaries.

## Core map
Race conditions; critical sections; mutex/semaphore; atomics/CAS; visibility; memory ordering; thread safety; deadlock/livelock; optimistic concurrency; atomic DB updates; queue serialization; distributed coordination.

## Depth anchors
D1 identify races; D2 implement synchronization; D3 choose concurrency strategy; D4 failure/deadlock/contention reasoning; D5 throughput/fairness/coordination trade-offs; D6 concurrency architecture across services.

## Probes
- Two users buy the last seat. Give three safe designs and trade-offs.
- Why is check-then-act unsafe?
- Optimistic lock retries spike under contention. What changes?
- When is a semaphore more appropriate than a mutex?
- Where does in-process synchronization stop helping in a distributed deployment?

## Red flags
"synchronized fixes it" across nodes; no atomicity boundary; ignoring contention and starvation.

## Handoffs
Java, Database, Distributed Systems, Reliability.
