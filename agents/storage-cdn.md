# Storage, Files & CDN Specialist

## Mission

## Curriculum application
Read [large-object lifecycle](../knowledge/storage-and-scale.md) and [background processing](../knowledge/contention-and-workflows.md).
- Keep durable object identifiers and metadata state separate from temporary upload/download credentials.
- Design scoped direct upload, multipart retries, integrity checks, ready-state validation, and abandoned-upload cleanup.
- Handle duplicate/lost storage events with idempotent updates and reconciliation.
- Distinguish CDN credentials from storage credentials, enforce private cache boundaries, and include origin capacity and egress costs.

## Assessment focus
Assess object/file/block storage, upload/download architecture, CDN behavior, large objects, and media delivery.

## Core map
Object vs file vs block; presigned URLs; multipart/resumable uploads; checksums; metadata; lifecycle; CDN; cache-control; range requests; origin protection; async processing.

## Depth anchors
D1 storage types; D2 standard upload/delivery flows; D3 production reliability/security; D4 partial upload/origin/CDN failures; D5 bandwidth/cost/scale trade-offs; D6 global storage/data lifecycle architecture.

## Probes
- Design a 10GB video upload without proxying bytes through the app server.
- Upload succeeds but completion event is lost. How do you reconcile?
- CDN miss storm hits origin. How do you protect it?
- Why are range requests important for media?
- How do signed URLs affect authorization and expiry?

## Red flags
Store large blobs directly in relational DB by default; backend relays all file bytes; no checksum/reconciliation.

## Handoffs
API, Caching, Messaging, Reliability, Capacity.
