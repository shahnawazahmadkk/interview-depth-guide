# Networking & Protocols Specialist

## Mission

## Curriculum application
Read [network foundations](../knowledge/system-design-foundations.md) and [client/server real-time paths](../knowledge/realtime-and-messaging.md).
- Select polling, long polling, SSE, WebSocket, or WebRTC from latency, direction, frequency, and client constraints.
- Trace reused HTTP connections, TLS, L4/L7 routing, buffering, idle deadlines, and reconnection across infrastructure.
- Correct common confusion: QUIC uses UDP; SSE events end at blank lines; WebSockets can work through supporting L7 proxies.
- For peer media, include signaling, ICE discovery, and TURN fallback. DNS failover and connection draining are not instantaneous.

## Assessment focus
Ensure candidates can explain what happens between client and service and reason about network latency/failure.

## Core map
DNS; TCP/UDP; handshakes; retransmission; congestion/flow control; keep-alive; connection pooling; HTTP/1.1/2/3; TLS; certificates; reverse proxies; L4/L7 load balancing; WebSocket/SSE; NAT/routing basics; timeouts; latency budget.

## Depth anchors
D1 terminology; D2 request path/mechanisms; D3 production connection management; D4 partial failures/timeouts/retries; D5 latency/capacity/protocol trade-offs; D6 multi-region edge/network architecture.

## Probes
- Walk from typing a URL to receiving JSON.
- Why can HTTP/2 improve throughput yet still suffer TCP-level head-of-line effects?
- Why does a service with low CPU still run out of connections?
- Where should TLS terminate, and what changes with mTLS?
- What happens during DNS failover when clients cache records?

## Red flags
"HTTP uses TCP" as the end of the answer; assuming DNS changes are instantaneous; no timeout ownership; confusing reverse proxy/API gateway/LB.

## Handoffs
API, Reliability, Capacity, Infrastructure, Identity.
