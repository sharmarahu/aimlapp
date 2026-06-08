# Use Cases

AIMLAPP is **AI-native project & scrum management** with a team-chat and
knowledge layer built in. Below are the most common ways teams run on it. Every
plan runs on one AI — **DeepSeek (V3 + R1)** — at a flat 1 credit per request.

---

## 1. Engineering teams running agile sprints  *(the core)*

**Who:** Software teams running Scrum or Kanban who want less ritual overhead.

**How it works:**
- Create a board, plan a sprint, and let the **AI Scrum Master** run the cadence.
- It opens daily standups, collects async answers, nudges non-responders, and posts a synthesised digest to the team huddle.
- Mid-sprint it forecasts whether you'll land the commitment and suggests what to descope; it sweeps stale tickets and flags duplicates.
- At sprint end it runs the retro, clusters themes, and **files the top actions back as tickets**.

**Features used:** AI Scrum Master · 7-column board + sprints · predictability reports (say/do, carryover, cycle time) · huddles.

**Outcome:** The rituals run themselves, the metrics are honest (snapshotted at close), and nothing falls through the cracks.

---

## 2. Startups & small teams  *(free forever)*

**Who:** Founders and small teams who need one place to plan work and talk.

**How it works:**
- Start free at [aimlapp.com/start](https://aimlapp.com/start) — up to **15 people**, your own `company.aimlapp.com`, no credit card.
- Plan on the board, chat in huddles + DMs, draft with AI, and let the AI Scrum Master keep standups moving.

**Features used:** Full free workspace · board · huddles/DMs · AI chat · canvases · memory.

**Outcome:** A complete, AI-run workspace at zero cost until you outgrow it.

---

## 3. Multi-team organisations  *(portfolio view)*

**Who:** Orgs running several squads who need a roll-up above the boards.

**How it works:**
- Group work under **initiatives → epics → milestones** with cross-board dependencies and per-member capacity.
- Each team gets its own board (`org` / `team` / `private` visibility), with a Scrum Master role separate from org admin.

**Features used:** Portfolio layer · multi-team boards · capacity planning · Scrum Master role.

**Outcome:** Squad autonomy with leadership visibility into delivery and risk.

---

## 4. Distributed & async teams

**Who:** Remote/timezone-spread teams where live standups don't work.

**How it works:**
- Standups are **async** — everyone answers on their own time; the AI assembles the digest.
- Huddles, DMs, @mentions with an inbox, scheduled send, presence/status, and Do-Not-Disturb keep communication tidy across timezones.

**Features used:** Async AI standups · huddles/DMs · mentions inbox · scheduled send · DND.

**Outcome:** Alignment without a synchronous meeting on the calendar.

---

## 5. Knowledge work with AI chat, RAG & memory

**Who:** Anyone who wants grounded AI alongside their work, not in a separate tab.

**How it works:**
- Attach a URL or paste text — it's ingested via **RAG** and queryable in natural language.
- Keep a living **canvas** per huddle and a private **memory** the AI can recall from.
- Use slash commands and the AI-assist toolbar (rewrite / shorten / translate / summarise) inline, with reusable **personas**.

**Features used:** RAG · canvases · personal memory · personas.

**Outcome:** AI that's grounded in *your* material and context, not just its training data.

---

## 6. Enterprises with security & compliance needs

**Who:** Larger orgs with IT/security requirements.

**How it works:**
- Connect **Active Directory / Microsoft Entra / Okta** via **SCIM** so users are provisioned and de-provisioned automatically; enforce **SSO** for the company domain.
- Turn on **2FA**, **DLP/PII masking**, **custom retention**, and review the **immutable audit log** (CSV export). Export org data on demand (GDPR).
- Optionally route AI through your **own DeepSeek key** (encrypted at rest).

**Features used:** SSO + SCIM/AD · 2FA · audit log · DLP · retention · data export · BYO key.

**Outcome:** Enterprise IAM and governance without leaving the tool your team already works in.

---

## 7. API integration for custom workflows

**Who:** Developers embedding AIMLAPP into their own products or automations.

**How it works:**
- Call the REST API with Bearer-token auth — `/api/chat` (DeepSeek), RAG ingestion, scrum, orgs.
- Org-scoped service-account tokens (scoped, IP-allow-listed) for machine-to-machine use.

**Features used:** REST API · org API tokens · SCIM endpoint.

**Outcome:** AIMLAPP as the AI-and-project backend for your own product — one prepaid provider, no rate-limit juggling.

---

## Feature-to-use-case matrix

| Feature | Eng/Scrum | Startup | Multi-team | Async | Knowledge | Enterprise |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| AI Scrum Master | ✅ | ✅ | ✅ | ✅ | | ✅ |
| Board + sprints + reports | ✅ | ✅ | ✅ | ✅ | | ✅ |
| Portfolio (epics/milestones) | ✅ | | ✅ | | | ✅ |
| Huddles / DMs / mentions | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| AI chat + RAG + memory | ✅ | ✅ | | | ✅ | ✅ |
| SSO + SCIM / AD | | | ✅ | | | ✅ |
| Audit log / DLP / retention | | | ✅ | | | ✅ |
| REST API + service tokens | ✅ | | | | ✅ | ✅ |
