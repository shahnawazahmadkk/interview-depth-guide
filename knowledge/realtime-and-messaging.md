# Real-time Delivery and Messaging

Solve two paths independently: client connection and source-to-connection-server propagation. Choosing WebSocket only solves part of the problem.

## Client delivery

| Approach | Useful baseline | Required reasoning |
|---|---|---|
| Periodic polling | Seconds of delay are acceptable | Polling QPS, cursor/query, keep-alive, jitter |
| Long polling | Infrequent updates should arrive promptly | Outstanding request state, proxy deadlines, timeout/reissue, missed-event cursor |
| SSE | Frequent one-way updates or generated-text streaming | Flush/buffering, event IDs, replay window, heartbeat |
| WebSocket | Frequent two-way messages | Application framing, authorization, heartbeat, reconnect, ordering |
| WebRTC | Interactive media or justified peer communication | Signaling, ICE, relay capacity, central persistence if needed |

Polling at interval T adds up to approximately T of detection delay before request/processing latency. Long polling removes most idle wait but adds request turnaround between responses. Batch any accumulated events rather than dropping intermediate changes.

SSE uses `text/event-stream`; a blank line terminates an event. A TCP packet or HTTP chunk is not an event boundary. EventSource supports reconnect and a last event ID, but the application must implement replay or snapshot recovery. Connection lifetimes depend on infrastructure; no universal 30-60 second expiry exists. HTTP/2 does not use HTTP/1.1 chunked transfer encoding. See [SSE framing and reconnection](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events).

WebSocket needs an application message contract, preferably with type, entity identifier, sequence, and operation identifier. Use secure transport for public clients. On disconnect, stop listeners and release connection resources. Bound output buffers; disconnect, coalesce, or resynchronize slow consumers according to product semantics. A connection can remain open across many messages, but deployments and networks still break it.

## Source propagation

### Stored updates plus polling

Persist updates, then query after a durable cursor. This keeps authoritative state centralized and simplifies recovery, at the cost of read volume and latency. One million clients polling every 10 seconds generate 100,000 requests per second before retries.

### Ownership routing

Map an entity, such as a collaborative document, to an owning server when rebuilding its working state is expensive. A registry/discovery layer distributes versioned membership; consistent hashing reduces reassignment on scaling. Track user sessions separately from document ownership when they differ.

During migration, coordinate old/new owners, drain clients, and transfer or reload state. Redirect stale senders and deduplicate any dual-delivered updates. Enforce ownership epochs or fencing where two writers could corrupt state. Hash placement alone does not guarantee exclusive ownership or durable storage.

### Pub/sub fan-out

Connect clients to lightweight endpoint servers. Each endpoint tracks local topic-to-connection mappings and subscribes only while needed. Publishers address a user, room, or entity channel; endpoint servers forward authorized updates to their local clients. Model multiple devices and unregister subscriptions when the last local listener leaves.

Redis Pub/Sub is ephemeral and at-most-once: disconnected subscribers lose messages. Use durable history or another replay mechanism when loss is unacceptable. See [Redis delivery semantics](https://redis.io/docs/latest/develop/use-cases/pub-sub/).

Do not treat a Kafka topic per user as a free substitute for lightweight pub/sub channels. Kafka topics/partitions carry broker overhead; consumer groups distribute partition work rather than broadcasting every record to every group member. Choose topic and partition granularity from expected counts, throughput, ordering, and operational limits.

## Queues, retained logs, and processing

A work queue hands tasks to competing consumers and normally removes acknowledged work. A retained log supports independent positions and rereads within retention. DLQ redrive of failed tasks is different from replaying all historical events. An event-sourced system uses its event history as authoritative state; simply publishing events does not establish event sourcing.

Specify partition keys, ordering scope, acknowledgment timing, replication/durability settings, retention, and replay. Processing frameworks add windows, state, event-time handling, watermarks, and late-data policy; those are not automatically properties of the transport broker.

Scale consumers according to available parallel work and downstream capacity. Ordered partition processing limits parallelism. A queue absorbs temporary bursts, not a permanent arrival/service deficit. Lag can result from a hot partition, slow dependencies, retries, rebalances, large records, or insufficient workers.

## Recovery and scale probes

- Reconnect: use bounded exponential backoff with jitter, a last acknowledged sequence, and replay or snapshot recovery. Avoid a simultaneous reconnection storm after a deployment.
- Ordering: choose per-room or per-entity ordering if sufficient. A single sequencer/partition simplifies total order within that scope. Vector clocks express causal relationships, not a total order by themselves.
- Popular publisher: distribute through regional fan-out, shared caches, batching, and selective subscriptions. Measure bytes and messages per recipient, not only inbound requests.
- Rolling deployment: stop accepting connections, drain with a deadline, reconnect progressively, and preserve durable history. A health check cannot migrate an existing TCP connection.
- Presence: define heartbeat expiry and stale-presence tolerance; a connected transport is not a durable proof that a person is active.
