# Security

AIML App is built with a security-first architecture. Every layer — authentication, authorization, data access, rate limiting, and credit management — is designed to fail closed rather than fail open.

---

## Authentication

### Session Tokens
- Sessions are **48-byte cryptographically secure random tokens** generated with `crypto.getRandomValues()`
- Tokens are stored in Cloudflare KV (`SESSIONS_KV`) with a **4-hour idle TTL**
- The TTL is a sliding window: any authenticated request resets the 4-hour clock (debounced to every 15 minutes to minimize KV writes)
- Tokens are transmitted exclusively via the `Authorization: Bearer` header — never via cookies, URL parameters, or response bodies after initial issuance
- On logout, the token is immediately deleted from KV

### Password Hashing
- New passwords are hashed with **PBKDF2-SHA256 at 100,000 iterations** with a unique salt per user
- A legacy SHA-256 path exists for backward compatibility with pre-migration accounts — it is read-only and cannot create new accounts
- Plaintext passwords are never logged or stored

### OAuth
- Google and Microsoft OAuth 2.0 are supported
- OAuth state parameters are stored in KV with a short TTL and deleted immediately after use — preventing CSRF and replay attacks
- No OAuth tokens are stored server-side after the initial exchange

### Password Reset
- Reset tokens are single-use, stored in KV with a **1-hour TTL**
- Tokens are delivered via email only (never in API responses)
- Tokens are deleted from KV immediately upon use
- A password reset bumps a `sessions_revoked_at` marker, so any leaked sessions die instantly

### Two-factor & passwordless
- **TOTP 2FA** — users can enrol an authenticator app (QR code) from their security page; login then requires a 6-digit code
- **Magic-link sign-in** — passwordless email link, single-use, 15-minute TTL

### Enterprise identity (SSO + SCIM / Active Directory)
- **SSO** via SAML / OIDC — employees sign in with their corporate identity (Microsoft Entra, Okta, etc.). Org admins can **enforce SSO** so password login is rejected for the company domain.
- **SCIM 2.0 provisioning** — connect your **Active Directory / Microsoft Entra / Okta** and users are **created, updated, and de-provisioned automatically**. When someone is removed in AD, they're deactivated here on the next sync. Set up self-service from the org admin panel: generate a scoped SCIM token, paste it plus the SCIM Tenant URL into your IdP. Tokens are SHA-256 hashed at rest, shown once, scoped, and revocable.
- **Org-scoped API tokens** — service accounts with capability scopes and optional IP allow-lists.

---

## Authorization

### Role Hierarchy
```
super_user  (hardcoded — cannot be modified or deleted via any API)
    └── admin  (platform administrator)
            └── org_admin  (organization administrator)
                    └── member  (standard user)
```

### Super User Protection
A single, hardcoded super-user account has absolute protection at the code level:

- **Undeletable** — `deleteUser` rejects any attempt to delete this account
- **Unmodifiable** — `changeUserRole`, `manageUserCredits`, `extendSubscription` all reject operations targeting this account
- **Hidden** — this account never appears in admin user lists, analytics queries, or any UI table
- **Hardcoded** — the check is in `isSuperUser()` in the Config class, intentionally not configurable via environment variables

Every admin endpoint that operates on user accounts must call `isSuperUser()` before acting.

### Admin Access
The analytics dashboard accepts two authentication methods:
1. A valid session token from an `admin`-role account
2. The `X-Admin-Key` header (a server-side secret set via `wrangler secret`)

If neither is present, access is denied. If `ADMIN_KEY` is not configured, the endpoint **fails closed** — no unauthenticated access is ever permitted.

### Organization Authorization
- Platform admins can operate on any org
- Org admins can only modify their own org's members and settings
- Members can only read their own org info
- All org operations validate membership before acting

---

## Rate Limiting

Rate limiting uses a **sliding-window counter** in `RATE_LIMIT_KV`:

| Plan | Requests / Minute |
|---|---|
| Free | 10 |
| Pro | 60 |
| Business | 150 |
| Enterprise | 300 |

- Limits are **per-user**, not per-IP (no proxy bypasses)
- The counter is checked and incremented atomically
- On KV failure, the system **fails closed** — the request is denied rather than allowed through

---

## Credit System

The credit system prevents runaway AI usage and provides per-request cost accountability.

- Credits are deducted with an **atomic SQL `UPDATE ... WHERE credits_remaining >= cost`** — there is no race condition where two concurrent requests could both see sufficient credits
- If the LLM call fails after credit deduction, credits are **automatically refunded** in the same request handler
- Every credit transaction (deduction, addition, refund) is written to `credit_audit_log` with a timestamp, reason, and performer
- Plan expiry is checked **per-request** — an expired plan immediately downgrades to Free tier without waiting for a cron job
- Organizations have a shared `credits_pool` with an optional per-member `credits_quota`. When a member's quota is exhausted, they cannot use the org pool even if the pool has remaining credits

---

## CORS

- CORS headers are generated from an **explicit allowlist** of origins in `Config.getCorsHeaders()`
- Wildcard (`*`) origins are never used in production
- CORS headers are generated **per-request** — module-level CORS headers would cause race conditions in the Cloudflare Workers runtime and are explicitly avoided
- OPTIONS preflight requests are handled on every endpoint

---

## Data Handling

### Chat Data Retention
- All chat sessions expire after **24 hours of inactivity** (`updated_at` based)
- On expiry, the session and all its messages, reactions, and room memberships are **hard-deleted** from D1 — no soft deletes, no archival
- A Cloudflare Cron Worker runs hourly to proactively purge expired sessions

### RAG Data Retention
- RAG sessions (ingested documents) auto-expire after **8 hours** via KV TTL
- Documents can be manually destroyed at any time via `POST /api/rag { action: 'destroy' }`
- Vector embeddings are stored in Cloudflare Vectorize scoped by `sessionId` — destroyed when the session is deleted

### Session Data
- Auth session tokens expire after **4 hours of inactivity** and are auto-deleted by KV TTL
- No session data is persisted to D1 — KV is the sole session store

### What We Don't Store
- No plaintext passwords
- No OAuth access tokens (discarded after exchange)
- No payment card data (billing handled externally)
- No user message content in logs (only token counts and model names are logged)

---

## Compliance & data rights

- **Immutable audit log** — every privileged action (role changes, member removal, branding, deletions, scrum events) is written to an append-only audit log. Org admins can **CSV-export** it.
- **DLP / PII masking** — optional regex-based redaction of sensitive data before it reaches the model.
- **Configurable retention** — per-org and per-channel retention windows.
- **Tenant isolation** — every read/write is scoped by `org_id` at the SQL layer, enforced (not just hidden). Private huddles are membership-gated at the data layer.
- **GDPR / data rights** — one-click **org data export** (full JSON of the tenant) and **self-service account deletion** (guarded by password or 2FA; org owners must transfer first so a tenant is never orphaned). Removed members are **anonymised** across shared content rather than leaving dangling identities.
- **BYO LLM key** — enterprises can route AI through their own DeepSeek key, **encrypted at rest** with AES-GCM and per-org key derivation.
- **Cost ceilings** — AI is single-provider and **prepaid** (hard cap), plus per-user/per-org credit limits and a global daily AI budget breaker — a surprise AI bill is impossible.

---

## Infrastructure Security

### Edge Network
- The application runs on **Cloudflare's global edge network** (300+ points of presence)
- DDoS protection is provided by Cloudflare's network layer — no additional configuration required
- TLS 1.3 is enforced for all connections

### Secrets Management
- All API keys (LLM providers, Resend, OAuth clients) are stored as **Wrangler secrets** — encrypted at rest by Cloudflare, never in environment files or source code
- Secrets are never logged or included in error responses

### Database
- Cloudflare D1 is a managed SQLite database — no exposed connection strings, no open ports
- All DB access goes through the Cloudflare Workers runtime binding — external direct access is impossible

### Isolation
- Each request runs in its own V8 isolate — no shared memory between requests
- KV and D1 operations are atomic by design

---

## Security Contact

Found a vulnerability? Please report it responsibly via **[aimlapp.com/contact](https://aimlapp.com/contact)** or **security@aimlapp.com**.

Include:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Your suggested fix (optional)

We aim to respond within 48 hours and resolve critical issues within 7 days.
