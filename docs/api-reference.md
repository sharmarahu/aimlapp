# API Reference

All API endpoints are available at `https://aimlapp.com/api`. Every request that modifies data or accesses user information requires a valid session token in the `Authorization` header.

## Authentication

```
Authorization: Bearer <session_token>
```

Session tokens are returned on login/register and expire after **4 hours of inactivity** (sliding window — activity resets the timer). All tokens are 48-byte cryptographically secure random strings.

---

## Endpoints

### Auth — `/api/auth`

#### Register
```http
POST /api/auth
Content-Type: application/json

{
  "action": "register",
  "email": "user@example.com",
  "password": "SecurePassword123",
  "name": "Jane Smith"
}
```

**Response**
```json
{
  "ok": true,
  "data": {
    "session_token": "tok_...",
    "user_id": "u_abc123",
    "name": "Jane Smith",
    "email": "user@example.com",
    "role": "user",
    "expiresAt": 1715000000000
  }
}
```

---

#### Login
```http
POST /api/auth
Content-Type: application/json

{
  "action": "login",
  "email": "user@example.com",
  "password": "SecurePassword123"
}
```

**Response** — same shape as register.

---

#### Google OAuth
```http
POST /api/auth
Content-Type: application/json

{ "action": "google_oauth", "origin": "https://aimlapp.com" }
```

**Response**
```json
{ "ok": true, "data": { "auth_url": "https://accounts.google.com/o/oauth2/..." } }
```
Redirect the user to `auth_url`. On return, the platform issues a session token automatically.

---

#### Forgot Password
```http
POST /api/auth
Content-Type: application/json

{
  "action": "forgot_password",
  "email": "user@example.com",
  "origin": "https://aimlapp.com"
}
```

Sends a password-reset link to the user's email. Link expires in 1 hour.

---

#### Get Profile
```http
GET /api/auth?action=profile
Authorization: Bearer <token>
```

**Response**
```json
{
  "ok": true,
  "data": {
    "user_id": "u_abc123",
    "name": "Jane Smith",
    "email": "user@example.com",
    "role": "user",
    "credits_remaining": 150,
    "plan": "pro",
    "plan_expires_at": "2025-06-01T00:00:00Z"
  }
}
```

---

#### Logout
```http
DELETE /api/auth
Authorization: Bearer <token>

{ "action": "logout", "session_token": "<token>" }
```

---

### Chat — `/api/chat`

The core AI endpoint. Supports streaming and non-streaming responses, session history, real-time rooms, RAG context, and meeting minutes generation.

#### Send a Message
```http
POST /api/chat
Authorization: Bearer <token>
Content-Type: application/json

{
  "message": "Explain transformer attention mechanisms",
  "model": "deepseek",
  "session_id": "<your_session_token>",
  "room_id": "room_abc123",
  "history": [
    { "role": "user", "content": "What is a neural network?" },
    { "role": "assistant", "content": "A neural network is..." }
  ]
}
```

**Parameters**

| Field | Type | Required | Description |
|---|---|---|---|
| `message` | string | ✅ | The user message |
| `model` | string | ✅ | Model key (see [Model Reference](#model-reference)) |
| `session_id` | string | ✅ | Your session token (also used as room ID) |
| `room_id` | string | optional | Explicit room ID for collaborative rooms |
| `history` | array | optional | Previous conversation turns for context |
| `rag_session_id` | string | optional | Active RAG session ID for document context |
| `user_name` | string | optional | Display name shown in collaborative rooms |
| `reply_to` | string | optional | Message ID to thread-reply to |
| `mentions` | array | optional | Array of `{ userId, name }` to mention |

**Response**
```json
{
  "ok": true,
  "data": {
    "message": "Transformer attention mechanisms work by...",
    "model": "deepseek",
    "tokens_used": 312,
    "credits_remaining": 149,
    "ai_msg_id": "msg_xyz789",
    "session_id": "room_abc123"
  }
}
```

---

#### Get Chat History
```http
GET /api/chat?room_id=room_abc123&limit=50
Authorization: Bearer <token>
```

**Response**
```json
{
  "ok": true,
  "data": {
    "messages": [
      {
        "id": "msg_001",
        "role": "user",
        "content": "Hello",
        "user_name": "Jane",
        "created_at": "2025-05-12T10:00:00Z"
      },
      {
        "id": "msg_002",
        "role": "assistant",
        "content": "Hello! How can I help?",
        "model": "deepseek",
        "created_at": "2025-05-12T10:00:01Z"
      }
    ],
    "room_id": "room_abc123",
    "member_count": 3
  }
}
```

---

#### Generate Meeting Minutes (MoM)
```http
POST /api/chat
Authorization: Bearer <token>
Content-Type: application/json

{
  "action": "generate_mom",
  "room_id": "room_abc123",
  "recipient_email": "team@example.com"
}
```

The platform reads the full room transcript, sends it to an LLM, generates structured Meeting Minutes, and emails them to the recipient.

---

#### Delete Chat Session
```http
DELETE /api/chat
Authorization: Bearer <token>
Content-Type: application/json

{ "room_id": "room_abc123" }
```

---

### RAG — Document Intelligence — `/api/rag`

#### Ingest a URL
```http
POST /api/rag
Authorization: Bearer <token>
Content-Type: application/json

{
  "action": "ingest",
  "url": "https://docs.example.com/api-guide"
}
```

**Response**
```json
{
  "ok": true,
  "data": {
    "session_id": "rag_abc123",
    "chunks_stored": 42,
    "message": "Document ingested. Use session_id in chat requests."
  }
}
```
The document is scraped, split into 150-word chunks (30-word overlap), embedded via Workers AI, and stored in Vectorize. Sessions auto-expire after 8 hours.

---

#### Query with RAG Context
Pass the `rag_session_id` in your chat request. The platform automatically retrieves the top-5 most relevant chunks and injects them as context before the user's message.

```http
POST /api/chat
Authorization: Bearer <token>
Content-Type: application/json

{
  "message": "What authentication methods does the API support?",
  "model": "deepseek",
  "session_id": "room_abc123",
  "rag_session_id": "rag_abc123"
}
```

---

#### Destroy RAG Session
```http
POST /api/rag
Authorization: Bearer <token>
Content-Type: application/json

{ "action": "destroy", "session_id": "rag_abc123" }
```

---

### Organizations — `/api/orgs`

#### Create Organization _(platform admin only)_
```http
POST /api/orgs
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "action": "create_org",
  "name": "Acme AI Team",
  "max_seats": 25,
  "plan": "enterprise",
  "subscription_until": "2026-01-01",
  "credits_pool": 50000,
  "description": "Acme Corporation's internal AI research team"
}
```

**Response**
```json
{
  "ok": true,
  "data": {
    "org_id": "org_abc123",
    "subscription_id": "SUB-XYZ123ABC456",
    "name": "Acme AI Team",
    "slug": "acme-ai-team",
    "plan": "enterprise",
    "max_seats": 25,
    "credits_pool": 50000
  }
}
```

---

#### Invite Member
```http
POST /api/orgs
Authorization: Bearer <org_admin_token>
Content-Type: application/json

{
  "action": "invite_member",
  "email": "newmember@acme.com",
  "role": "member"
}
```

An email is sent with a 7-day invite link. The recipient clicks it and is automatically added to the org.

---

#### List Org Members
```http
GET /api/orgs?action=members
Authorization: Bearer <org_admin_token>
```

**Response**
```json
{
  "ok": true,
  "data": {
    "members": [
      {
        "user_id": "u_abc",
        "name": "Jane Smith",
        "email": "jane@acme.com",
        "role": "org_admin",
        "credits_used": 1240,
        "credits_quota": 5000,
        "joined_at": "2025-04-01T00:00:00Z"
      }
    ],
    "org": {
      "name": "Acme AI Team",
      "plan": "enterprise",
      "credits_pool": 50000,
      "credits_used": 12400,
      "max_seats": 25,
      "subscription_id": "SUB-XYZ123ABC456"
    }
  }
}
```

---

#### Change Member Role
```http
POST /api/orgs
Authorization: Bearer <org_admin_token>
Content-Type: application/json

{
  "action": "change_role",
  "target_user_id": "u_abc",
  "new_role": "org_admin"
}
```

---

#### Set Member Credit Quota
```http
POST /api/orgs
Authorization: Bearer <org_admin_token>
Content-Type: application/json

{
  "action": "set_member_quota",
  "target_user_id": "u_abc",
  "credits_quota": 5000
}
```

---

#### Start Silent Standup
```http
POST /api/orgs
Authorization: Bearer <org_admin_token>
Content-Type: application/json

{
  "action": "start_standup",
  "title": "Monday Standup",
  "send_mom": true
}
```

Creates a shared room and notifies all org members. When `send_mom: true`, ending the standup triggers automatic Meeting Minutes generation and email delivery.

---

#### End Standup
```http
POST /api/orgs
Authorization: Bearer <org_admin_token>
Content-Type: application/json

{
  "action": "end_standup",
  "room_id": "room_standup123"
}
```

---

### Analytics — `/api/analytics` _(admin only)_

Requires either a valid admin session token or the `X-Admin-Key` header.

#### Dashboard Summary
```http
GET /api/analytics?action=dashboard
Authorization: Bearer <admin_token>
```

**Response**
```json
{
  "ok": true,
  "data": {
    "total_users": 81,
    "active_sessions": 12,
    "total_requests": 857,
    "timestamp": "2025-05-12T10:00:00Z"
  }
}
```

---

#### Usage Stats
```http
GET /api/analytics?action=usage&start_date=2025-05-01&end_date=2025-05-12
Authorization: Bearer <admin_token>
```

---

#### User Management
```http
GET /api/analytics?action=users
Authorization: Bearer <admin_token>
```

Returns paginated user list with credit balances, plan info, and last activity. The super-user account is never included in this list.

---

#### Add Credits to User
```http
POST /api/analytics
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "action": "add_credits",
  "user_id": "u_abc123",
  "amount": 500,
  "reason": "Customer support goodwill"
}
```

---

#### Extend Subscription
```http
POST /api/analytics
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "action": "extend_subscription",
  "user_id": "u_abc123",
  "days": 30
}
```

---

## Model Reference

AIMLAPP is single-provider: **DeepSeek**, with a flat **1 credit per request** for every model. Pass the vendor name in `model` and the specific version in `model_version`.

| `model` | `model_version` | Description | Credits/Request |
|---|---|---|---|
| `deepseek` | `deepseek-chat` | DeepSeek V3 — fast general chat (default) | 1 |
| `deepseek` | `deepseek-reasoner` | DeepSeek R1 — step-by-step reasoning | 1 |

> Requests for any other provider are transparently routed to DeepSeek. Embeddings for RAG/memory use Cloudflare Workers AI (`bge-base-en-v1.5`) — a separate, free path.

---

## Error Codes

| HTTP Status | Code | Description |
|---|---|---|
| 400 | `VALIDATION_ERROR` | Missing or invalid request field |
| 401 | `UNAUTHORIZED` | Missing or expired session token |
| 402 | `INSUFFICIENT_CREDITS` | User has 0 credits remaining |
| 403 | `FORBIDDEN` | Role does not permit this action |
| 404 | `NOT_FOUND` | Resource does not exist |
| 409 | `CONFLICT` | Duplicate resource (e.g. email already registered) |
| 410 | `SESSION_EXPIRED` | Chat session expired (24h inactivity) |
| 429 | `RATE_LIMITED` | Too many requests (see plan limits) |
| 500 | `INTERNAL_ERROR` | Server error — credits refunded automatically |

**Error Response Shape**
```json
{
  "ok": false,
  "error": {
    "code": "INSUFFICIENT_CREDITS",
    "message": "You have no credits remaining. Please upgrade your plan."
  }
}
```

---

## Rate Limits

| Plan | Requests / Minute |
|---|---|
| Free | 10 |
| Pro | 60 |
| Business | 150 |
| Enterprise | 300 |

Rate limiting uses a sliding-window counter in Cloudflare KV. Exceeding the limit returns HTTP 429. Limits are per-user, not per-IP.
