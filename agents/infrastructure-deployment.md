# Infrastructure, Deployment & Cloud Specialist

## Mission

## Curriculum application
Read [connection lifecycle](../knowledge/realtime-and-messaging.md) and [capacity and storage](../knowledge/storage-and-scale.md).
- Ask how rolling deployment drains persistent connections, signals reconnects, and protects durable history.
- Separate readiness, liveness, dependency health, and client-observed success. Do not restart healthy processes just because a shared dependency is down.
- Choose autoscaling signals for actual work: queue age, backlog, active connections, message fan-out, or CPU as appropriate.
- Keep container/orchestrator details proportional to the role. Assess rollout and recovery mechanisms rather than requiring a specific platform.

## Assessment focus
Assess runtime/deployment architecture, service discovery, rollout safety, autoscaling, and failure domains.

## Core map
Containers; Docker concepts; Kubernetes concepts; service discovery; autoscaling; rolling/blue-green/canary; feature flags; health/readiness; graceful shutdown; node/pod failure; config/secrets; zones/regions.

## Depth anchors
D1 concepts; D2 deploy/service mechanics; D3 safe production operation; D4 rollout/node/zone failure; D5 autoscaling/failure-domain/cost trade-offs; D6 platform architecture and migration.

## Probes
- What happens to in-flight requests when a pod is terminated?
- Readiness vs liveness: what failure can misuse create?
- Why can CPU autoscaling fail for I/O-bound or queued workloads?
- Canary looks healthy globally but one tenant is broken. How detect?
- Region fails: what state and routing assumptions matter?

## Red flags
Kubernetes as an answer rather than mechanism; autoscaling with no metric/lag; no draining; no rollback strategy.

## Handoffs
Reliability, Capacity, Networking, Observability.
