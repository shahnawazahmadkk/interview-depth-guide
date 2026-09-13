# Generic Design Practice Catalog

Difficulty is a starting estimate for the exercise, not the candidate's demonstrated depth or a company's hiring standard. These prompts are generic practice seeds, not full answer keys. No company attribution is implied. Choose one from the learner's current gaps; reveal follow-ups progressively.

| Exercise | Starting difficulty | Core focus | Follow-up to test depth |
|---|---|---|---|
| URL shortener | Easy | Identifier generation, redirect lookup | Collision handling, hot links, abuse |
| File synchronization service | Easy | Blob/metadata split, versioning | Conflicting edits, resumable transfer |
| Local business search | Easy | Search filters, geospatial lookup | Ranking, stale indexes |
| Local delivery service | Easy | Regional data, order lifecycle | Moving couriers, dispatch contention |
| Event ticketing | Medium | Inventory, search, reservations | Hot event, expiry versus payment race |
| Photo-sharing service | Medium | Media upload, feed retrieval | Fan-out, privacy, CDN invalidation |
| Social news feed | Medium | Follow graph, feed generation | Popular publishers, hybrid fan-out |
| Matching application | Medium | Profiles, location, mutual matching | Duplicate likes, privacy, hotspots |
| Coding practice platform | Medium | Job execution, status, limits | Untrusted code isolation, fairness |
| Messaging application | Medium | Persistent delivery, history | Ordering, reconnect, multi-device |
| Activity tracking | Medium | Location ingestion, history | Offline sync, geospatial privacy |
| Distributed cache | Medium | Placement, eviction, replication | Node loss, hot keys, rebalancing |
| Rate limiter | Medium | Counters, windows, atomicity | Distributed enforcement, clock skew |
| Online auction | Medium | Bid ordering, contention | Late bids, reconnect, finalization |
| Video sharing | Medium | Upload, processing, CDN | Transcoding backlog, partial failure |
| Job scheduler | Medium | Durable jobs, claiming, retries | Worker crash, duplicate effects |
| Live comments | Medium | Pub/sub, fan-out | Celebrity load, batching, slow consumers |
| News aggregation | Medium | Ingestion, deduplication, feeds | Source failures, freshness |
| Price tracking | Medium | Crawling, change detection | Quotas, notifications, missed updates |
| Notification service | Medium | Preferences, channels, retries | Deduplication, provider outage |
| Top-K videos | Hard | Aggregation, windows, sketches | Approximation error, late events |
| Ride dispatch | Hard | Nearby search, matching | Regional failure, exclusive driver claims |
| Trading application | Hard | Order state, market updates | Ordering, idempotency, reconciliation |
| Collaborative document editor | Hard | Session routing, concurrent edits | Convergence, ownership migration |
| Web crawler | Hard | Frontier, deduplication, politeness | Bloom-filter false positives, recovery |
| Ad-click aggregation | Hard | Streams, windows, deduplication | Late events, replay, accounting accuracy |
| Social post search | Hard | Inverted indexes, permissions | Freshness, deletion, hot terms |
| Payment service | Hard | State transitions, idempotency | Unknown outcomes, reconciliation |
| Metrics monitoring | Hard | Time windows, retention, histograms | Cardinality, ingestion pressure, alert lag |
| Online board game | Hard | Authoritative moves, sessions | Simultaneous moves, reconnect, timers |
| Conversational AI service | Hard | Threads, streamed generation | Cancellation, quotas, retrieval access |
| Flash-sale inventory | Hard | Admission, atomic claims | Extreme contention, fairness, overselling |
| Food review application | Medium | Reviews, media, discovery | Moderation, rating aggregation |
| Game leaderboard | Medium | Ranking, score updates | Ties, seasonal resets, hot ranks |
| Donation platform | Hard | Payment lifecycle, receipts | Duplicate submissions, campaign bursts |
| CI workflow runner | Hard | DAG scheduling, isolated jobs | Secrets, retries, artifact retention |

## Practice loop

1. State a few functional requirements, explicit invariants, and latency/scale assumptions.
2. Have the candidate attempt a complete design before showing a reference approach.
3. Review the read/write paths, failure behavior, and the most important trade-off.
4. Drill one weak mechanism with a changed constraint.
5. Repeat under a time limit with the candidate narrating the design.

A useful starting milestone is three easy, three medium, and two hard attempts, adjusted to time and target depth. Count completed attempts separately from mastery. Repeating a corrected design with a new failure condition is stronger evidence than reading its solution.

## Optional adjacent preparation

Coding stays opt-in. When requested, assess two pointers, sliding windows, intervals, stacks, linked lists, binary search, heaps, DFS/BFS, backtracking, graphs, dynamic programming, greedy choices, tries, prefix sums, and matrices. Practice complexity, edge cases, and narration, including occasional work without code execution. A topic list is not proof of mastery.

Behavioral preparation also stays opt-in. Use anonymized experience summaries covering scope, ownership, ambiguity, perseverance, conflict resolution, growth, communication, and leadership. Structure answers around context, action, result, and learning. Probe the candidate's own decisions, measurable outcomes, and reflection without inventing experiences. Keep behavioral evidence distinct from technical D0-D6 scores.
