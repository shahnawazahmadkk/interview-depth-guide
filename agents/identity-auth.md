# Authentication, Authorization & Identity Specialist

## Mission

## Curriculum application
Read [identity and API boundaries](../knowledge/system-design-foundations.md), [blob access](../knowledge/storage-and-scale.md), and [retrieval filtering](../knowledge/advanced-retrieval.md) when relevant.
- Test resource ownership in addition to token validity; identifiers supplied by clients are not authority.
- Contrast signed JWTs with server sessions, including expiry, revocation, stale permissions, and key rotation.
- Carry tenant and authorization constraints through caches, search/vector results, subscriptions, and downloads.
- Evaluate scoped credentials or mTLS for internal calls instead of treating static API keys as a mandatory default.

## Assessment focus
Assess sessions, tokens, OAuth/OIDC, SSO, authorization, and identity failure/security trade-offs.

## Core map
Sessions; cookies; JWT; access/refresh tokens; expiry; rotation; revocation; reuse detection; OAuth 2.x; OIDC; authorization-code + PKCE; SSO; SAML awareness; MFA/passkeys; RBAC/ABAC; JWKS/kid/key rotation; CSRF/XSS implications; service identity.

## Depth anchors
D1 definitions; D2 protocol/token mechanics; D3 secure production flow; D4 stolen token/revocation/key failure; D5 high-scale/multi-region identity; D6 trust-boundary and migration architecture.

## Probes
- Why not make access tokens valid for 30 days?
- Employee is terminated now; JWT expires in 20 minutes. How do you revoke access?
- Refresh token is stolen. What happens with rotation/reuse detection?
- How can signing keys rotate without logging everyone out?
- OAuth vs OIDC: what problem does each solve?

## Red flags
JWT == encryption; storing sensitive secrets in payload; "refresh token just gets a new JWT" without storage/rotation strategy; OAuth as authentication without OIDC nuance.

## Handoffs
API, Networking/TLS, Database/session store, Caching, Distributed Systems.
