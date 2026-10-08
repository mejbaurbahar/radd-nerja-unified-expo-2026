# Deep Audit of Radd AI & Nerja AI + Complete Open-Source & Market Compendium

**Author:** Senior QA Engineer & Technical Systems Architect  
**Prepared for:** Ammar Ahmed & Executive Engineering Leadership  
**Date:** October 8, 2026  
**Audience:** The Hajj Conference & Exhibition 2026 Executive Team  

---

# SECTION 1: DEEP TECHNICAL AUDIT OF BOTH PLATFORMS

## 1. RADD-AI DEEP AUDIT

### A. Environment & Infrastructure Topology
From `/Users/a1/Desktop/Office/QA-Projects/Radd-AI/credentials.md` and live audits:
* **Production Dashboard:** `https://app.radd-ai.com/en/dashboard`
* **Development CRM:** `https://dev.radd-ai.com`
* **AI Agent Frontend:** `https://dev-aiagent.prowhats.com` (Next.js)
* **AI Agent Backend API:** `https://dev-api-aiagent.prowhats.com` (FastAPI / Python)
* **Compute Layer:** Serverless AWS Lambda (1024 MB allocated memory).
* **Database Layer:** Serverless PostgreSQL, auto-scaling from 0.5 vCPUs up to 64 vCPUs.
* **Cache Layer:** Redis Cluster (2 vCPUs, 2 GB RAM) for real-time contact session state and message deduplication.

### B. Live Platform State & Database Records (`https://app.radd-ai.com`)
* **Session:** Authenticated as Super Admin `ammarahmed…`.
* **Tenant Volume:** 713 companies registered in database (latest: Company #713 `سبسبسشيب`, #712 `omar test`, #711 `منصة سلم للوظائف`, #710 `nocoji`, #709 `sabbirhossen`, #708 `رواء العزيزيه`, #707 `جمعية كفلاء`, #705 `جمعية ايادي`).
* **Active Platform Metrics:**
  - Registered Users: 91
  - Paying Clients: 1 (Enterprise trial)
  - Modules Verified:
    1. **Companies Management (`/en/companies`):** Tenant configuration, user seating, assigned WhatsApp phone numbers.
    2. **Broadcasts Engine (`/en/broadcasts`):** Meta WhatsApp template message distribution.
    3. **Promo Codes (`/en/promo-codes`):** Discount and affiliate referral tracking.
    4. **Subscriptions (`/en/subscriptions`):** Billing tiers, MRR/ARR tracking.
    5. **WhatsApp Migration (`/en/whatsapp-migration`):** Automated tooling for moving phone numbers from standard WhatsApp Business to official Meta Cloud API WABAs.
    6. **Settings & Localization (`/en/settings`):** Language switcher (Arabic/English), webhook listeners.

### C. Known Technical Findings & Automated Test Suite (Playwright)
From `/Users/a1/Desktop/Office/QA-Projects/Radd-AI/test-cases.md`:
* 31 automated Playwright test cases (`automation/tests/radd.spec.js`) covering authentication, navigation, and tenant controls.
* **Critical Finding (TC-082):** Language switch to Arabic translates text and mirrors visuals, but `<html dir="ltr">` remains `"ltr"` instead of setting `"rtl"`. Must be patched prior to Hajj Expo to ensure native Arabic text rendering on mobile browsers.
* **Critical Finding (TC-050/051):** Voice Agent Call / Recent Calls endpoint encountered 401 unauthorized errors under legacy token headers.

---

## 2. NERJA-AI DEEP AUDIT

### A. Codebase Architecture (`/Users/a1/Desktop/Office/nerja/`)
Nerja AI is an enterprise monorepo composed of 4 key sub-systems:
1. **`nerjaai-backend` (NestJS / TypeScript):**
   - Implements modular architecture: `packages/api/src/shopify`, `packages/api/src/tracker`, `packages/api/src/campaigns`.
   - Database: PostgreSQL with Prisma ORM + Redis.
   - PR #69 recently merged extending token TTL to 30 mins and retaining store parameters on connection drops.
2. **`nerjaai-frontend` (Next.js 15 App Router / Tailwind CSS):**
   - Modern dashboard (`/app`, `/app/data-room`, `/app/analytics`, `/app/revenue`, `/app/leads`, `/app/campaigns`).
   - PR #81 merged introducing resilient error boundaries and auto-retry for Shopify merchant connections.
3. **`nerjaai-ecommerce` (Storefront Integrations):**
   - Connectors for Shopify App Store (`apps.shopify.com/nerjaai-commerce`) and WooCommerce.
4. **`nerjaai-python` (Machine Learning & Predictive Engine):**
   - Computes real-time intent scores based on clickstream velocity and cart dwell time.

### B. Live Platform State & NeuralTag Tracker (`https://nerja.ai/app/data-room`)
* **Live Tenant:** Connected to production store `aura-market.ai.studio`.
* **NeuralTag Snippet:** 
  `https://f5976ye542.execute-api.ap-south-1.amazonaws.com/t.js?key=nak_pk_live_dBsm&api=https://9pl1zg5yte.execute-api.ap-south-1.amazonaws.com`
* **Real-Time Event Stream:** 3,779+ live user interactions captured.
* **DOM Heuristics Active:**
  - `Hover Product`
  - `Hover Add To Cart`
  - `Wishlist Add`
  - `Add To Cart`
  - `Scroll Milestone`
  - `Interaction`

---

# SECTION 2: EXHAUSTIVE OPEN-SOURCE & MARKET COMPENDIUM

Here are all the verified open-source repositories, official documentation links, tech stacks, and CodeCanyon commercial scripts relevant to Ammar Ahmed's vision.

---

## 1. OMNICHANNEL SHARED INBOXES & CUSTOMER SUPPORT PLATFORMS

### 1. **Chatwoot** (The Industry Standard Open-Source Shared Inbox)
* **GitHub Repository:** [https://github.com/chatwoot/chatwoot](https://github.com/chatwoot/chatwoot)
* **Official Website & Docs:** [https://www.chatwoot.com](https://www.chatwoot.com) · [https://www.chatwoot.com/docs](https://www.chatwoot.com/docs)
* **GitHub Metrics:** ~20,000 Stars · 3,500+ Forks · Active Commits (October 2026)
* **License:** MIT / AGPLv3 Dual License
* **Tech Stack:**
  - Backend: Ruby on Rails 7
  - Frontend: Vue.js 3 + Tailwind CSS
  - Real-time: ActionCable WebSockets + Redis
  - Database: PostgreSQL
* **Key Features:**
  - Multi-channel support out of the box: WhatsApp (Meta Cloud API), Website Live Chat, Instagram, Facebook Messenger, Twitter/X, Telegram, Email, SMS (Twilio).
  - Multi-agent collaboration: Private notes, team assignments, canned responses.
  - Webhooks & REST API for CRM sync.
* **Comparison to Our Platform:** Chatwoot is purely a **reactive customer support inbox**. It does **not** have client-side DOM behavioral intent auto-capture (like NerjaTag) and has **no** native in-chat e-commerce checkout.

---

### 2. **Typebot** (Conversational Form & Flow Builder)
* **GitHub Repository:** [https://github.com/baptisteArno/typebot.io](https://github.com/baptisteArno/typebot.io)
* **Official Website & Docs:** [https://typebot.io](https://typebot.io) · [https://docs.typebot.io](https://docs.typebot.io)
* **GitHub Metrics:** ~13,500 Stars · 2,200+ Forks
* **License:** AGPLv3
* **Tech Stack:**
  - Full-stack: Next.js / TypeScript
  - Styling: Chakra UI / Tailwind
  - Database: PostgreSQL with Prisma ORM
* **Key Features:**
  - Visual drag-and-drop conversational form builder.
  - Direct WhatsApp channel embedding via WhatsApp Cloud API.
  - Native integrations: OpenAI, Google Sheets, Make, Zapier, Webhooks.
* **Comparison to Our Platform:** Excellent for lead-capture surveys and static questionnaire flows, but lacks autonomous AI agent reasoning and dynamic ERP inventory synchronization (such as hotel rooms or water trucks).

---

### 3. **Botpress** (Enterprise Conversational AI Platform)
* **GitHub Repository:** [https://github.com/botpress/botpress](https://github.com/botpress/botpress)
* **Official Website & Docs:** [https://botpress.com](https://botpress.com) · [https://botpress.com/docs](https://botpress.com/docs)
* **GitHub Metrics:** ~13,000 Stars
* **License:** AGPLv3 (v12) / Cloud API
* **Tech Stack:**
  - Engine: Node.js / TypeScript
  - NLU: Hybrid LLM + Intent Classifier
  - UI: React / Redux
* **Key Features:**
  - Autonomous LLM agents with knowledge base embeddings (RAG).
  - Multi-channel routing: WhatsApp, Slack, Teams, Messenger.
* **Comparison to Our Platform:** Heavy developer framework requiring extensive configuration; does not include built-in multi-tenant billing or e-commerce tracking.

---

### 4. **Rasa** (Contextual Open-Source Conversational AI)
* **GitHub Repository:** [https://github.com/RasaHQ/rasa](https://github.com/RasaHQ/rasa)
* **Official Website & Docs:** [https://rasa.com](https://rasa.com) · [https://rasa.com/docs/rasa](https://rasa.com/docs/rasa)
* **GitHub Metrics:** ~19,500 Stars
* **License:** Apache 2.0 (Core) / Polyform Perimeter
* **Tech Stack:** Python 3.10+ / PyTorch / spaCy
* **Key Features:**
  - High enterprise security and on-premise data sovereignty.
  - Deterministic state machine coupled with probabilistic LLM generation.
* **Comparison to Our Platform:** Best for banks and governments requiring 100% on-premise execution, but high engineering overhead and no turnkey WhatsApp dashboard.

---

### 5. **Chaskiq** (Conversational Marketing & Support Suite)
* **GitHub Repository:** [https://github.com/chaskiq/chaskiq](https://github.com/chaskiq/chaskiq)
* **Official Website:** [https://chaskiq.io](https://chaskiq.io)
* **License:** AGPLv3
* **Tech Stack:** Ruby on Rails / React.js / PostgreSQL / Redis
* **Key Features:** Open-source alternative to Intercom / Drift. Includes automated visitor onboarding tours, in-app messaging, and WhatsApp connectors.

---

## 2. WHATSAPP GATEWAYS & API PROTOCOLS

### 1. **WAHA (WhatsApp HTTP API)**
* **GitHub Repository:** [https://github.com/devlikeapro/waha](https://github.com/devlikeapro/waha)
* **Official Documentation:** [https://waha.devlike.pro](https://waha.devlike.pro)
* **License:** MIT (Core) / Commercial (Plus)
* **Tech Stack:** Node.js / TypeScript / Docker / NestJS
* **Key Features:**
  - Wraps WhatsApp Web sockets (Baileys/Puppeteer) into a clean, RESTful HTTP API with Swagger documentation.
  - Supports sending text, media, buttons, and listening via webhooks.

### 2. **Evolution API**
* **GitHub Repository:** [https://github.com/EvolutionAPI/evolution-api](https://github.com/EvolutionAPI/evolution-api)
* **Official Documentation:** [https://doc.evolution-api.com](https://doc.evolution-api.com)
* **License:** Apache 2.0
* **Tech Stack:** Node.js / Express / Redis / Baileys
* **Key Features:** Multi-instance WhatsApp gateway popular among Latin American and Middle Eastern developers for multi-tenant SaaS architectures.

> ⚠️ **Critical Architectural Note:** Both WAHA and Evolution API rely on reverse-engineered WhatsApp Web sockets. For **The Hajj Conference 2026** and enterprise deployments, **Radd AI's use of the Official Meta Cloud API** is strictly required to guarantee 99.99% uptime and prevent WhatsApp account bans.

---

## 3. BEHAVIORAL ANALYTICS & INTENT AUTO-TRACKING (NERJATAG BENCHMARKS)

### 1. **PostHog** (Open-Source Product Analytics & Auto-Capture)
* **GitHub Repository:** [https://github.com/PostHog/posthog](https://github.com/PostHog/posthog)
* **Official Documentation:** [https://posthog.com/docs](https://posthog.com/docs)
* **GitHub Metrics:** ~24,000 Stars · 1,500+ Contributors
* **License:** MIT
* **Tech Stack:**
  - Ingestion: Node.js / Rust / Kafka
  - Storage: ClickHouse (Columnar OLAP DB for billions of events) + PostgreSQL
  - UI: React / TypeScript
* **Key Features:**
  - **DOM Auto-Capture:** Injects a single `<script>` snippet into `<head>` that automatically captures clicks, inputs, pageviews, and scroll events without writing tracking code.
  - Session replay, heatmaps, and funnel analytics.
* **Direct Relevance to NerjaTag:** NerjaTag's heuristic auto-detection operates on the exact same principles as PostHog auto-capture, but Nerja adds an e-commerce semantic layer (extracting product price, currency, and cart attributes directly from DOM elements).

### 2. **RudderStack** (Open-Source Customer Data Platform & Event Pipeline)
* **GitHub Repository:** [https://github.com/rudderlabs/rudder-server](https://github.com/rudderlabs/rudder-server)
* **Official Documentation:** [https://www.rudderstack.com/docs](https://www.rudderstack.com/docs)
* **License:** AGPLv3
* **Tech Stack:** Go (Golang) / Node.js / PostgreSQL / Redis
* **Key Features:** Segment-compatible open-source event routing engine. Ingests events from web, iOS, and Android SDKs and routes them to 150+ analytics and warehouse destinations.

### 3. **Plausible Analytics**
* **GitHub Repository:** [https://github.com/plausible/analytics](https://github.com/plausible/analytics)
* **Official Documentation:** [https://plausible.io/docs](https://plausible.io/docs)
* **Tech Stack:** Elixir / Phoenix / ClickHouse
* **Key Features:** Lightweight (<1 KB script), privacy-first, cookie-less web analytics.

---

## 4. CONVERSATIONAL AI ORCHESTRATION & AGENT WORKFLOWS

### 1. **n8n** (Fair-Code Workflow Automation)
* **GitHub Repository:** [https://github.com/n8n-io/n8n](https://github.com/n8n-io/n8n)
* **Official Documentation:** [https://docs.n8n.io](https://docs.n8n.io)
* **GitHub Metrics:** ~49,000 Stars
* **Tech Stack:** TypeScript / Node.js / Vue.js
* **Key Features:** Advanced node-based workflow automation. Features native LangChain / AI Agent nodes that can connect WhatsApp incoming messages directly to Nerja intent APIs.

### 2. **Dify.ai** (LLM Application & AI Agent Platform)
* **GitHub Repository:** [https://github.com/langgenius/dify](https://github.com/langgenius/dify)
* **Official Documentation:** [https://docs.dify.ai](https://docs.dify.ai)
* **GitHub Metrics:** ~52,000 Stars
* **Tech Stack:** Python (Flask/FastAPI) / Next.js / PostgreSQL / Redis
* **Key Features:** Turnkey multi-agent orchestrator with built-in RAG, prompt engineering studio, and multi-channel publishing.

---

## 5. CODECANYON / CODEINIAN COMMERCIAL SCRIPTS (MARKET BENCHMARK)

These are the exact PHP/Laravel SaaS scripts Ammar referred to during the meeting:

| Script Name | Marketplace | Tech Stack | Pricing | Capabilities & Vulnerabilities |
| :--- | :--- | :--- | :--- | :--- |
| **WaDesk** | CodeCanyon | PHP / Laravel 10 / Vue.js | $59 - $299 (Extended) | WhatsApp CRM SaaS with OpenAI ChatGPT auto-reply. Monolithic architecture; uses unofficial web socket libraries; lacks enterprise SLA. |
| **WhatsMine / Wapi** | CodeCanyon | PHP / Laravel / MySQL | $49 - $199 | Multi-tenant WhatsApp marketing & broadcasting tool with subscription billing. No e-commerce auto-capture. |
| **WhatsFood** | CodeCanyon | Laravel / Flutter | $69 | WhatsApp food and restaurant ordering tool with PDF digital menus. Static form based; no dynamic AI consultation. |
| **WhatsStore** | CodeCanyon | Laravel / Tailwind | $59 | WhatsApp catalog builder allowing shopkeepers to receive orders via WhatsApp text strings. No automated payment link reconciliation. |

---

# SECTION 3: STRATEGIC SYNTHESIS & WHY WE WIN

### The Critical Takeaway for Ammar Ahmed:
1. **Nobody else unites all three layers:**
   - Tools like **Chatwoot** only do *Support*.
   - Tools like **WaDesk / Wati** only do *Broadcasts*.
   - Tools like **PostHog / Plausible** only do *Analytics*.
2. **Our Proprietary Advantage:**
   - **Nerja AI** catches the visitor the second they hover or add an item to their cart on web or mobile.
   - **Radd AI** immediately fires an autonomous WhatsApp recovery conversation in their native language (Arabic/Indonesian).
   - **Mihad & Qatarat** fulfill the room booking, water delivery, or transport reservation with instant Mada payment links.

This closed-loop system is uniquely positioned to dominate the upcoming **Hajj Conference & Exhibition 2026** in Jeddah.
