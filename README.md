# AIML App — Enterprise AI Assistant Platform

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare" />
  <img src="https://img.shields.io/badge/Models-40%2B_LLMs-6366F1?style=for-the-badge" />
  <img src="https://img.shields.io/badge/License-MIT-10B981?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Production-22C55E?style=for-the-badge" />
</p>

**AIML App** is a production-ready, enterprise-grade AI assistant platform built on Cloudflare's global edge network. It gives your team access to 40+ leading AI models through a single, secure, credit-governed interface — with real-time collaboration, document intelligence (RAG), and full multi-tenant organization management built in.

🌐 **Live at**: [aimlapp.com](https://aimlapp.com)

---

## Why AIML App?

| Pain Point | AIML App Solution |
|---|---|
| Managing API keys for multiple AI providers | Single platform — Gemini, OpenAI, Anthropic, DeepSeek all in one |
| No visibility into AI usage and costs | Real-time analytics dashboard with per-user credit tracking |
| AI responses can't be verified | Built-in cross-model verification — check any response with a second model |
| Can't collaborate on AI tasks | Real-time shared chat rooms with WebSocket sync |
| Uploading docs to every AI tool separately | RAG pipeline — attach URLs/documents once, query with any model |
| Managing AI access across teams | Multi-tenant org system with seat limits, roles, and credit pools |

---

## Core Features

### 🤖 Multi-Model AI Chat
Access 40+ models from four providers in a single interface. Switch models per message. All requests are credit-governed with automatic refunds on failure.

**Supported Providers:**
- **Google Gemini** — Gemini 2.5 Flash, Gemini 2.0 Flash, Gemini 1.5 Pro/Flash
- **OpenAI** — GPT-4.1, GPT-4o, GPT-4 Turbo, o1, o3-mini
- **Anthropic** — Claude Opus 4.7, Claude Sonnet 4.5, Claude Haiku 4.5
- **DeepSeek** — DeepSeek V3, DeepSeek R1

### 🔍 Cross-Model Verification
Every AI response has a built-in **Verify** button. Select any other model to independently review and critique the response — eliminating single-model blind spots.

### 📄 Document Intelligence (RAG)
Attach any public URL and AIML App scrapes, chunks, embeds, and indexes it automatically. Then query your documents in natural language using any AI model. Sessions expire after 8 hours — no data hoarding.

### ⚡ Real-Time Collaborative Rooms
Invite teammates into shared AI chat rooms. Everyone sees messages in real time via WebSocket (Cloudflare Durable Objects). The AI participates in the group conversation. Perfect for pair-programming, research sessions, and team decision-making.

### 🏢 Multi-Tenant Organizations
Platform admins create organizations with seat limits, plan tiers, subscription expiry, and a shared credit pool. Org admins manage members and quotas without touching the platform admin dashboard.

### 🤫 Silent Standups & Meeting Minutes
Org admins can start a silent standup room. When it ends, an LLM automatically generates structured Meeting Minutes (MoM) and emails them to every participant.

### 📊 Admin Analytics
Real-time dashboard showing total requests, active sessions, user plans, credit consumption, email delivery logs, and model usage breakdowns.

---

## Quick Start

### For End Users
1. Visit [aimlapp.com](https://aimlapp.com) and register
2. You get **20 free credits** on signup (no credit card required)
3. Select a model, type your question, get an answer
4. Use the **Verify** button to cross-check responses with a second model
5. Attach a URL for document-grounded answers

### For Organizations
1. Contact us to provision your org
2. You receive a `SUB-XXXXXXXXXXXX` subscription ID
3. Your org admin invites members by email
4. Members get access to a shared credit pool with optional per-member quotas

### For Developers
See the [API Reference](docs/api-reference.md) to integrate AIML App into your own workflows and applications.

---

## Documentation

| Document | Description |
|---|---|
| [API Reference](docs/api-reference.md) | All endpoints, request/response schemas, authentication |
| [Use Cases](docs/use-cases.md) | Industry-specific use cases and workflows |
| [Security](docs/security.md) | Security architecture, auth model, data handling |
| [Organizations & Multi-Tenancy](docs/multi-tenancy.md) | Org management, roles, credit pools |
| [RAG — Document Intelligence](docs/rag.md) | How to use document-grounded AI |
| [Real-Time Chat Rooms](docs/realtime.md) | Collaborative rooms and WebSocket architecture |
| [Pricing & Plans](docs/pricing.md) | Plan tiers, credits, and billing model |

---

## Architecture Overview

```
User Request
    │
    ▼
Cloudflare Pages (Edge, 300+ PoPs globally)
    │
    ▼
functions/index.js  ←  Main router
    │
    ├── /api/chat       ← Multi-model LLM inference + streaming
    ├── /api/auth       ← Auth: register, login, OAuth, password reset
    ├── /api/orgs       ← Multi-tenant organization management
    ├── /api/rag        ← RAG: scrape → embed → query
    ├── /api/analytics  ← Admin dashboard
    ├── /api/messages   ← Direct messages
    └── /api/bookmarks  ← Saved responses
         │
         ▼
    External LLM APIs (Gemini / OpenAI / Anthropic / DeepSeek)
    Cloudflare D1 (SQLite)
    Cloudflare KV (sessions, cache, rate limits)
    Cloudflare Vectorize (RAG vector search)
    Cloudflare Workers AI (embeddings)
    Cloudflare Durable Objects (WebSocket rooms)
    Resend (transactional email)
```

---

## Technology Stack

| Layer | Technology |
|---|---|
| Runtime | Cloudflare Workers (V8 isolates, edge-native) |
| Database | Cloudflare D1 (SQLite) |
| Sessions & Cache | Cloudflare KV |
| Vector Search | Cloudflare Vectorize (768-dim cosine) |
| Embeddings | Cloudflare Workers AI (`bge-base-en-v1.5`) |
| Real-Time | Cloudflare Durable Objects + WebSocket |
| Email | Resend |
| Auth | PBKDF2-SHA256 + session tokens, Google OAuth, Microsoft OAuth |

---

## License

MIT — see [LICENSE](LICENSE)

---

<p align="center">Built on Cloudflare's global edge network · Serving users at <a href="https://aimlapp.com">aimlapp.com</a></p>
