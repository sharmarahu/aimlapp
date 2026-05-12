# Organizations & Multi-Tenancy

AIML App supports full multi-tenant organization management. A single platform deployment serves multiple independent organizations, each with its own members, credit pool, seat limits, and subscription timeline.

---

## Concepts

### Organization
A named group of users with a shared credit pool, seat limit, and subscription. Each org receives a unique `subscription_id` (`SUB-XXXXXXXXXXXX`) generated at creation time — this ID is shown in the org dashboard and can be used for billing reference.

### Roles
| Role | Scope | Permissions |
|---|---|---|
| `admin` | Platform-wide | Create/manage all orgs, view all analytics, manage all users |
| `org_admin` | One org | Invite/remove members, change roles, set quotas, update org settings |
| `member` | One org | Access shared credit pool up to their quota, view own org info |

A user can only belong to one organization at a time.

### Credit Pool
Each org has a `credits_pool` — a shared budget that all members draw from. When a member sends a chat request, credits are deducted from both the member's individual usage counter and the org pool. The pool is set by the platform admin and can be topped up at any time.

### Per-Member Quotas
Org admins can set a `credits_quota` per member — a hard cap on how many credits that individual can consume from the pool. Once a member hits their quota, they are blocked from further requests until the quota is raised or reset.

---

## Org Lifecycle

### 1. Platform Admin Creates Org
```
POST /api/orgs
{
  "action": "create_org",
  "name": "Acme AI Team",
  "max_seats": 25,
  "plan": "enterprise",
  "subscription_until": "2026-01-01",
  "credits_pool": 100000
}
```

The org is created with a unique `subscription_id` and a `slug` derived from the name.

### 2. Org Admin Invites Members
The org admin sends invite links via:
```
POST /api/orgs
{ "action": "invite_member", "email": "user@acme.com", "role": "member" }
```

- An email is sent with a **7-day invite link**
- The link contains a secure token stored in D1
- When the user clicks the link, they are prompted to register (if new) or log in, then automatically added to the org

### 3. Members Join
Members navigate to `/join-org?token=<invite_token>`, complete authentication, and are added to the org. A confirmation email is sent to both the new member and all org admins.

### 4. Org Admin Manages Members
- **Change role**: Promote a member to `org_admin` or demote back to `member`
- **Set quota**: Assign a per-member credit limit
- **Remove member**: Removes the user from the org immediately; they revert to a standard free-plan user

### 5. Subscription Expiry
When `subscription_until` passes, org members' plans expire on their next request. They are automatically downgraded to Free tier. The platform admin can extend the subscription at any time via the analytics dashboard.

---

## Email Notifications

All org lifecycle events trigger transactional emails via Resend:

| Event | Recipients |
|---|---|
| Member invited | Invited email address |
| Member joined | New member + all org admins |
| Role changed | Affected member |
| Member removed | Removed member |
| Credit pool updated | All org admins |
| Member quota changed | Affected member |
| Org settings updated | All org admins |
| Standup started | All org members |
| Meeting Minutes ready | All standup participants |

---

## Silent Standups

Org admins can run asynchronous standups without a live meeting:

1. Admin starts a standup: `POST /api/orgs { "action": "start_standup", "title": "Monday Standup", "send_mom": true }`
2. All org members receive an email with a link to the standup room
3. Members join the room and type their updates (async — no need to be online at the same time)
4. Admin ends the standup: `POST /api/orgs { "action": "end_standup", "room_id": "..." }`
5. If `send_mom: true`, the platform reads the full transcript, sends it to Gemini 2.5 Flash, generates structured Meeting Minutes, and emails them to every participant

---

## Dashboard

Org admins access their dashboard at `/org`. It shows:
- Org name, plan, subscription expiry, `subscription_id`
- Credit pool balance and total credits used
- Member list with individual credit consumption and quotas
- Pending invitations
- Seat utilization (current members / max seats)

Platform admins see all orgs in the analytics dashboard at `/analytics`.

---

## Limits & Quotas

| Parameter | Set By | Default |
|---|---|---|
| `max_seats` | Platform admin | Defined at org creation |
| `credits_pool` | Platform admin | Defined at org creation |
| `credits_quota` per member | Org admin | None (uses full pool) |
| Invite expiry | System | 7 days |
| Plan tier (`free`, `pro`, `business`, `enterprise`) | Platform admin | Defined at org creation |
