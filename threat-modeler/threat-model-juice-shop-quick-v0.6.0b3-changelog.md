# Threat Model — Full Change Log

> Complete, uncapped audit trail of every assessment run for this threat
> model: all added, changed, and removed findings, mitigations, abuse
> cases, instances, and components. The report's own Change Log section is
> a summarized window of this history. Generated deterministically from
> `threat-model.yaml`; do not hand-edit.

## v1 — 2026-09-13 15:43 CEST · full · quick · sonnet-economy

_commit `6244c59` · 33 findings · delta basis: initial_

### Added
- **Findings (33):**
  - T-001 — Missing Audit Logging for Security Events
  - T-002 — Unmetered WebSocket Connections Enable Resource Exhaustion
  - T-003 — Hardcoded RSA private key enables JWT forgery — lib/insecurity.ts:23
  - T-004 — Insecure JWT Verification
  - T-005 — SQL injection in login query bypasses credential check — routes/login.ts:34
  - T-006 — SQL injection request data interpolated into a SQL string — routes/search.ts:23
  - T-007 — Insecure Direct Object Reference
  - T-008 — Hardcoded wallet mnemonic exposes derivable private key — routes/checkKeys.ts:10
  - T-009 — Mass assignment privileged field accepted from request — routes/verify.ts:52
  - T-010 — Password reset accepts a security-question answer — routes/resetPassword.ts:10
  - T-011 — Password predictably derived from email in OAuth — oauth.component.ts:30
  - T-012 — Unauthenticated WebSocket Channel
  - T-013 — NoSQL $where injection in product reviews — routes/showProductReviews.ts:36
  - T-014 — Path traversal filesystem access from request input — routes/dataErasure.ts:104
  - T-015 — Non-cryptographic RNG for a secret/token — lib/insecurity.ts:55
  - T-016 — XXE XML parsed with external entities enabled — routes/fileUpload.ts:83
  - T-017 — Input in executable NoSQL predicate — routes/trackOrder.ts:18
  - T-018 — Input compiled as template source — routes/userProfile.ts:87
  - T-019 — DOM XSS — search-result.component.ts:143
  - T-020 — Unauthenticated wallet injection corrupts challenge — routes/web3Wallet.ts:16
  - T-021 — MD5 password hashing without salt — lib/insecurity.ts:43
  - T-022 — JWT in localStorage exposed to XSS exfiltration — request.interceptor.ts:13
  - T-023 — Challenge-solved notifications broadcast to — registerWebsocketEvents.ts:31
  - T-024 — Unauthenticated LLM chat endpoint without rate limiting — server.ts:637
  - T-025 — Unbounded Set growth — routes/web3Wallet.ts:16
  - T-026 — Missing JWT algorithm allowlist allows HS256 confusion — lib/insecurity.ts:54
  - T-027 — Prompt injection bypasses coupon policy server-side — routes/chat.ts:177
  - T-028 — Input passed to code execution — routes/userProfile.ts:61
  - T-029 — Sensitive Routes Registered Without Authentication Middleware
  - T-030 — Missing ownership check enables cross-user — routes/updateProductReviews.ts:16
  - T-031 — Missing author identity check in review — routes/createProductReviews.ts:24
  - T-032 — Event loop blocking — routes/showProductReviews.ts:36
  - T-033 — AdminGuard enforces admin access client-side only — app.guard.ts:53
- **Mitigations (32):**
  - M-001 — Allowlist client-controlled fields
  - M-002 — Manual review: verify Password reset accepts a security-question answer
  - M-003 — Constrain file paths to a safe base directory
  - M-004 — Remove server-side evaluation of untrusted input
  - M-005 — Remove server-side evaluation of untrusted input
  - M-006 — Enforce server-side authorization on every endpoint
  - M-007 — Enforce server-side authorization on every endpoint
  - M-008 — Offload CPU-bound work and bound execution time
  - M-009 — Add security audit logging
  - M-010 — Rate-limit expensive requests and bound input size
  - M-011 — Move cryptographic keys to a managed secret store
  - M-012 — Enforce JWT signature and algorithm verification
  - M-013 — Use parameterized database queries
  - M-014 — Use parameterized database queries
  - M-015 — Enforce object-level (ownership) authorization
  - M-016 — Move secrets to a managed secret store
  - M-017 — Allowlist client-controlled fields
  - M-018 — Replace security-question recovery with a CSPRNG reset token delivered through a
  - M-019 — Harden the authentication flow
  - M-020 — Require authentication on every exposed endpoint
  - M-021 — Use parameterized database queries
  - M-022 — Constrain file paths to a safe base directory
  - M-023 — Use cryptographically secure random values
  - M-024 — Disable XML external entity (XXE) resolution
  - M-025 — Use parameterized database queries
  - M-026 — Remove server-side evaluation of untrusted input
  - M-027 — Encode output instead of bypassing the framework sanitizer
  - M-028 — Apply least-privilege filesystem access
  - M-029 — Hash passwords with a strong, salted algorithm
  - M-030 — Store session tokens in HttpOnly, Secure cookies
  - M-031 — Stop exposing internal information to clients
  - M-032 — Rate-limit expensive requests and bound input size
- **Components (8):** auth, backend, ci-cd-pipeline, database, frontend, marsdb-store, socketio, web3-nft

### Scope
- **Re-analyzed:** auth, backend, ci-cd-pipeline, database, frontend, marsdb-store, socketio, web3-nft

> first full scan
