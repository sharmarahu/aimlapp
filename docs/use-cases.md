# Use Cases

AIML App is a horizontal platform that adapts to how your team works. Below are the most common deployment patterns across industries.

---

## 1. Enterprise AI Assistant for Internal Teams

**Who:** Mid-to-large companies wanting to give all employees access to AI without managing API keys or billing per-person.

**How it works:**
- IT provisions an org with a shared credit pool (e.g. 100,000 credits/month)
- Employees get invited via email, no credit card needed
- Department heads set per-member credit quotas to prevent runaway usage
- The analytics dashboard gives IT a real-time view of usage by model and user

**Key features used:**
- Multi-tenant organizations with credit pools
- Per-member credit quotas
- Admin analytics dashboard
- 40+ models so different teams use what fits (e.g. finance uses Claude for reasoning, engineering uses DeepSeek for code)

**Outcome:** Centralized AI access, predictable costs, no shadow IT with individual API keys.

---

## 2. AI-Powered Customer Support

**Who:** SaaS companies that want their support agents to answer tickets faster using AI.

**How it works:**
- Agents open AIML App alongside their helpdesk
- They paste customer questions and get suggested responses
- They attach product documentation URLs via RAG — the AI answers only from official docs
- The **Verify** feature lets agents cross-check any answer with a second model before sending

**Key features used:**
- RAG (attach knowledge base URLs — docs, FAQs, changelogs)
- Cross-model verification (reduce hallucinations before sending to customer)
- Chat history (context carries across the session)

**Outcome:** Faster ticket resolution, fewer escalations, AI grounded in your actual documentation.

---

## 3. Research & Analysis Teams

**Who:** Analysts, consultants, or researchers who synthesize large volumes of information.

**How it works:**
- Paste report URLs or paste document text into RAG ingestion
- Ask follow-up questions across multiple documents in a single session
- Use high-reasoning models (o3-mini, Claude Opus, DeepSeek R1) for complex analysis
- Export or email the generated Meeting Minutes from research sessions

**Key features used:**
- RAG document intelligence (multiple URLs per session)
- Model selection (match model capability to task complexity)
- Meeting Minutes generation (email session summary to stakeholders)

**Outcome:** Hours of reading time compressed into minutes. Analysis grounded in the source material, not just training data.

---

## 4. Software Development Teams

**Who:** Engineering teams who pair-program with AI, review code, write tests, and draft technical documentation.

**How it works:**
- Developers join a shared collaborative room during code review or architecture sessions
- Everyone can see the AI's responses in real time — no copy-pasting back and forth in Slack
- DeepSeek V3/R1 for code generation, Claude Opus for architecture reviews, GPT-4.1 for documentation
- RAG is used to attach internal API specs or GitHub READMEs

**Key features used:**
- Real-time collaborative rooms (whole team + AI in one room)
- Multi-model selection (right model for each task)
- RAG (internal documentation, API specs)
- Thread replies (follow up on specific AI responses without losing context)

**Outcome:** AI pair-programming at the team level, not just individual level. Architecture decisions documented automatically.

---

## 5. Education & Training

**Who:** Training providers, bootcamps, and L&D teams who build learning experiences around AI.

**How it works:**
- Instructors create org-level access for cohorts of students
- Each cohort gets a shared credit pool with per-student quotas
- Students use AI to get explanations, generate practice problems, and check their work
- Instructors monitor usage via the analytics dashboard to see which topics generate the most AI queries

**Key features used:**
- Organizations with per-member credit quotas (controls usage per student)
- Analytics (understand where students struggle based on query volume)
- Free tier models (keep costs low for high-volume student usage)

**Outcome:** Scalable AI tutoring without individual student billing or API key management.

---

## 6. Consulting & Professional Services

**Who:** Consultants who deliver AI-powered insights to clients as a service.

**How it works:**
- Consultants use their AIML App org to run analysis during engagements
- Client-specific documents are loaded into RAG at the start of each engagement
- Deliverables are produced using the MoM generator after client workshops
- Consultants verify any critical recommendation with the cross-model Verify feature before presenting

**Key features used:**
- RAG (client documents, contracts, reports)
- Meeting Minutes (auto-generate workshop summaries)
- Cross-model verification (reduce risk on high-stakes recommendations)
- Org roles (junior consultants as members, senior consultants as org_admins)

**Outcome:** Faster delivery, documented workshops, reduced hallucination risk on client-facing output.

---

## 7. Product & Marketing Teams

**Who:** Non-technical teams that need AI for writing, research, and ideation but don't want to manage tools.

**How it works:**
- Marketers use Gemini 2.5 Flash for fast first drafts
- They verify brand-sensitive copy with Claude Opus
- Product managers attach competitor pages via RAG to ground competitive analysis
- Teams share rooms during campaign brainstorming sessions

**Key features used:**
- Multi-model (fast drafting + quality review in one tool)
- Cross-model verification
- RAG (competitor research, brand guidelines)
- Collaborative rooms (team brainstorming)

**Outcome:** AI-assisted content that goes through a structured verification step, reducing brand risk.

---

## 8. API Integration for Custom Workflows

**Who:** Developers who want to embed AIML App's capabilities into their own products or automation pipelines.

**How it works:**
- Use the REST API with Bearer token auth
- Build custom frontends that call `/api/chat` with any supported model
- Trigger RAG ingestion programmatically when documents change
- Use the analytics API to build internal usage dashboards

**Key features used:**
- Full REST API (chat, RAG, auth, orgs)
- All 40+ models available via API
- Credit system tracks usage automatically

**Outcome:** AIML App becomes the AI inference backend for your own product — without managing provider relationships or rate limits.

---

## Feature-to-Use-Case Matrix

| Feature | Enterprise | Support | Research | Dev Teams | Education | Consulting |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| Multi-model chat | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Cross-model verify | ✅ | ✅ | ✅ | ✅ | | ✅ |
| RAG document chat | ✅ | ✅ | ✅ | ✅ | | ✅ |
| Real-time rooms | ✅ | | ✅ | ✅ | ✅ | ✅ |
| Organizations | ✅ | ✅ | | ✅ | ✅ | ✅ |
| Credit pools & quotas | ✅ | ✅ | | | ✅ | ✅ |
| Meeting Minutes | ✅ | | ✅ | ✅ | | ✅ |
| Analytics dashboard | ✅ | ✅ | | ✅ | ✅ | |
| REST API | | | ✅ | ✅ | | |
