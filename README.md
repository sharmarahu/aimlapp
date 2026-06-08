# AIMLAPP — AI Project & Scrum Management

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare" />
  <img src="https://img.shields.io/badge/AI-DeepSeek_V3_%26_R1-7C3AED?style=for-the-badge" />
  <img src="https://img.shields.io/badge/License-MIT-10B981?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Production-22C55E?style=for-the-badge" />
</p>

> **An AI Scrum Master runs your team's rituals; an AI Project Manager runs the
> strategic layer above them.** Sprints, standups, retros, and reporting are the
> hero. Team chat, huddles, canvases, and memory are the conversation and
> knowledge layers that serve it.

🌐 **Live at [aimlapp.com](https://aimlapp.com)** · ▶️ **[Take the live tour](https://demo.aimlapp.com/app)** · 🆓 **[Start free](https://aimlapp.com/start)** — your whole team, no credit card.

---

## What is AIMLAPP?

AIMLAPP is an **AI-native project & scrum management platform**. Instead of a
board you have to babysit, you get an **AI Scrum Master** on staff: it opens your
daily standups, chases non-responders, writes the digest, runs your retro and
turns the takeaways into tickets, forecasts whether the sprint will land, and
flags stale or duplicate work — all with full audit trails and a hard cost cap.

Every company gets a **private, isolated workspace** at `your-company.aimlapp.com`
with its own branding, members, boards, and data. One brand (like Slack or
Linear) — not a rebrand-it-yourself white-label.

---

## ✨ Core features

### 🤖 AI Scrum Master *(shipped)*
Your autonomous agile facilitator:
- **Daily standups** — opens them on schedule, collects async answers, nudges non-responders, and posts an AI-synthesised digest to the team huddle.
- **Retrospectives** — collects "went well / went wrong / ideas", clusters themes with AI, and **files the top action items straight back as tickets** so retros aren't just talk.
- **Mid-sprint forecast** — burndown math vs. commitment; projects whether you'll finish and suggests what to descope.
- **Stale + duplicate detection** — sweeps neglected tickets and flags near-duplicate work before it ships twice.
- **AI estimates & assignment hints** — suggests story points and the best-fit assignee from history.
- **Safety rails by design** — every autonomous action is audit-logged, idempotent, kill-switchable, and capped by a daily AI budget.

### 📋 Full Scrum & Project board
- 7-column lifecycle (`draft → refined → ready → in progress → testing → pending → completed`), drag-and-drop, per-card menus.
- **Sprints** with start / lock / complete lifecycle and a one-active-sprint invariant.
- **Portfolio layer** — initiatives, epics (drag-rank), milestones, ticket dependencies, member capacity.
- **Predictability & reports** — committed-vs-delivered (say/do), carryover, scope-change, on-time %, cycle/lead time, burndown, and an AI sprint-review close-out. Metrics are snapshotted immutably at sprint close, so history never gets rewritten.
- **Scrum Master role** — granular control of rituals, sprint lock, and capacity, separate from org admin.
- Multi-team boards with per-board membership (`org` / `team` / `private` visibility).

### 💬 Team communication
- **Huddles** (channels) — public or private, membership enforced at the data layer.
- **Direct messages**, **@mentions** with an inbox, read receipts, scheduled send, presence/status, and Do-Not-Disturb.
- **AI chat** powered by **DeepSeek V3 & R1** — with slash commands, an AI-assist toolbar (rewrite / shorten / translate / summarise), and prompt **personas**.

### 🧠 Knowledge layer
- **RAG** — attach a URL or paste text; it's scraped, chunked, embedded, and made queryable in natural language.
- **Huddle Canvases** — a living markdown doc per huddle, re-indexed for AI retrieval.
- **Personal memory** — a private, embeddings-backed memory store the AI can recall from.

### 🏢 Multi-tenant by architecture
- Auto-provisioned subdomain per org (`<company>.aimlapp.com`) in seconds.
- **Hard isolation enforced at the database query layer** — not just list-hiding.
- Per-org branding (logo, accent, app name), feature toggles, credit pools, and per-member quotas.

### 🔐 Security, identity & compliance
- **SSO** (SAML / OIDC) and **SCIM 2.0 provisioning** — connect your **Active Directory / Microsoft Entra / Okta** so users are created *and de-provisioned* automatically. Self-service setup right in your org admin panel. → [docs/security.md](docs/security.md)
- **2FA (TOTP)** and passwordless **magic-link** sign-in.
- **Immutable audit log** of every privileged action, with CSV export.
- **DLP / PII masking**, configurable **data retention**, **member anonymisation** on removal.
- **GDPR**: one-click org data export and self-service account deletion.
- **BYO key** — enterprises can route AI through their own DeepSeek key, encrypted at rest (AES-GCM, per-org key derivation).

---

## 💸 Pricing — one AI, flat, no surprises

Every plan runs on **DeepSeek (V3 + R1) at a flat 1 credit per request**. Because
DeepSeek is **prepaid**, a runaway AI bill is impossible.

| Plan | Price | Team | AI credits / mo | Highlights |
|---|---|---|---|---|
| **Free** | $0 forever | up to 15 | 200 | Full workspace, AI Scrum Master, your own subdomain |
| **Pro** | $29/mo | up to 25 | 5,000 | Roles, audit log, per-member quotas, priority support |
| **Business** | $99/mo | up to 100 | 25,000 | DLP, custom retention, GDPR export, SLA-ready |
| **Enterprise** | Custom | unlimited | custom | SSO + SCIM, BYO key, compliance, dedicated SLA |

[Start free →](https://aimlapp.com/start) · full breakdown in [docs/pricing.md](docs/pricing.md)

---

## 🚀 Getting started

1. Go to **[aimlapp.com/start](https://aimlapp.com/start)** and create your free workspace (up to 15 people, no card).
2. You land on **`your-company.aimlapp.com`** — invite your team, create a board, link a huddle.
3. Turn on the **AI Scrum Master** and let it run your first standup.
4. *(Optional, Enterprise)* Connect your Active Directory via **SCIM** from the org admin panel for automatic user provisioning.

Prefer to look before you sign up? **[Take the live tour →](https://demo.aimlapp.com/app)**

---

## 📚 Documentation

| Doc | What's inside |
|---|---|
| [docs/use-cases.md](docs/use-cases.md) | Who it's for and how teams run on it |
| [docs/pricing.md](docs/pricing.md) | Plans, credits, and what a credit buys |
| [docs/security.md](docs/security.md) | Isolation, SSO/SCIM/AD, audit, DLP, retention, BYO keys |
| [docs/multi-tenancy.md](docs/multi-tenancy.md) | Per-org subdomains and data isolation |
| [docs/rag.md](docs/rag.md) | Document intelligence (RAG) pipeline |
| [docs/realtime.md](docs/realtime.md) | Real-time huddles and presence |
| [docs/api-reference.md](docs/api-reference.md) | API surface overview |

---

## 🛠️ Built on

**Cloudflare Workers + Pages** · **D1** (SQLite) · **KV** · **Vectorize** (embeddings) ·
**Durable Objects** (real-time) · **Workers AI** (embeddings) · **DeepSeek** (chat & reasoning) ·
**React 19 + Vite + Tailwind** (marketing). Single-provider, prepaid, cost-capped AI.

---

<p align="center"><sub>© AIMLAPP — AI Project &amp; Scrum Management · <a href="https://aimlapp.com">aimlapp.com</a></sub></p>
