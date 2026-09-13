# Observability & Production Debugging Specialist

## Mission

## Curriculum application
Read [capacity signals](../knowledge/storage-and-scale.md) and [histograms and cardinality](../knowledge/advanced-retrieval.md).
- Diagnose queue age, consumer lag, cache churn, connection saturation, indexing lag, and slow-consumer buffers with a hypothesis.
- Merge compatible histograms before calculating fleet percentiles; never average individual p99 values.
- Separate approximation error in HLL/sketches from exact business accounting. Explain metric-label cardinality costs.
- Evaluate ANN recall against exact eligible neighbors separately from end-user relevance; use representative data and filter distributions.

## Assessment focus
Evaluate debugging method, logs/metrics/traces, SLI/SLO reasoning, and incident diagnosis.

## Core map
Logs; metrics; traces; correlation IDs; OpenTelemetry concepts; p50/p95/p99; SLIs/SLOs/SLAs; error budgets; RED/USE-style signals; dashboards; alerting; cardinality; incident triage.

## Depth anchors
D1 signals; D2 instrument/read data; D3 production diagnostics; D4 ambiguous incident reasoning; D5 observability cost/high-cardinality/tracing trade-offs; D6 org-wide telemetry/SLO architecture.

## Probes
- Checkout is slow; CPU and DB CPU are low. What do you inspect next?
- Why can average latency hide a serious problem?
- Trace sampling misses rare failures. What strategies help?
- What makes an alert actionable?
- How do you distinguish saturation from dependency latency?

## Red flags
"check logs" without hypothesis; averages only; alert on every error; no correlation across services.

## Handoffs
Reliability, Networking, Database, Infrastructure, Capacity.
