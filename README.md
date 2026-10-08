# Pawan Laksahan

**Full Stack Software Engineer — .NET & AWS**

I build and operate production SaaS systems end to end: REST and event-driven backends in
C#/.NET, React and Next.js frontends, and the AWS infrastructure underneath them. Three-plus
years on a high-traffic recruitment platform have left me equally comfortable tuning a
saturated SQL Server, hardening an authentication flow, or putting an LLM behind a typed
interface in a message pipeline.

🎓 **AWS Certified Solutions Architect – Associate** (SAA-C03)

📧 [Email](mailto:pawanlaksahan10@gmail.com) · 💼 [LinkedIn](https://www.linkedin.com/in/pawanlaksahan-22a451209) ·
🚀 Live: [Nostalgia AI](https://nostalgia-ai-frontend.vercel.app) · [ELVI Music Studio](https://elvistudio.dpdns.org)

---

## Featured projects

### 🎬 Nostalgia AI — [**live demo →**](https://nostalgia-ai-frontend.vercel.app)

[`nostalgia-ai-backend`](https://github.com/PawanLaksahanOfficial/nostalgia-ai-backend) ·
[`nostalgia-ai-frontend`](https://github.com/PawanLaksahanOfficial/nostalgia-ai-frontend)

<p align="center">
  <img src="https://raw.githubusercontent.com/PawanLaksahanOfficial/nostalgia-ai-backend/main/docs/example-frame.jpg" alt="A frame from a generated video: a still lake lined with trees, the AI-written narration as captions, and a small 'Made with Nostalgia AI' watermark" width="640">
</p>

Turns a written memory and an optional photo into a narrated, captioned video. A background
worker runs the whole pipeline — **LLM script → voice-over → word-timed captions → stock
photos → FFmpeg composition → object storage** — while the client polls for progress. Around
it sits the production machinery that makes it a product rather than a demo: subscription
billing with idempotent Stripe webhooks, atomic per-plan quota enforcement, range-enabled media
streaming, and opaque share tokens with expiry and revocation so a recipient needs no account.

- **Abuse protection.** Every free video draws on one shared AI quota, so sign-ups pass layered
  checks: email confirmation, Gmail dot/`+tag` canonicalisation, a ~9,000-domain disposable-email
  blocklist, and rolling per-network and site-wide caps — with client IPs stored only as keyed
  one-way hashes.
- **Built for free tiers.** The live service runs on 0.1 CPU and 512 MB, so the pipeline renders
  one job at a time, falls back across a list of LLM models, and degrades gracefully — narrating
  the user's own text — when the AI is unavailable.
- **Clean Architecture.** Four projects with dependencies pointing inward (Domain → Application
  → Infrastructure → API) keep FFmpeg, storage, email, and payments at the edges. Repository
  tests run against a real EF Core context on SQLite, not mocks.

> **Backend** · .NET 8 · ASP.NET Core · EF Core 9 · PostgreSQL · OpenRouter · Edge TTS · FFmpeg ·
> Stripe · S3-compatible storage (Supabase / Cloudflare R2) · Brevo / AWS SES · Docker on Render ·
> 190+ xUnit tests
>
> **Frontend** · React 19 · TypeScript · Vite · Redux Toolkit · React Router · Google & Meta
> sign-in · Vitest + Testing Library · Vercel

### 🎸 ELVI Music Studio — [**live demo →**](https://elvistudio.dpdns.org)

[`Music-Studio-and-Instruments-Renting-Management-System`](https://github.com/PawanLaksahanOfficial/Music-Studio-and-Instruments-Renting-Management-System)
· one-click read-only demo login

<p align="center">
  <img src="https://raw.githubusercontent.com/PawanLaksahanOfficial/Music-Studio-and-Instruments-Renting-Management-System/main/docs/screenshots/rentals.png" alt="ELVI Music Studio's Product Rentals page: summary cards for rentals out, overdue, due today and unpaid, above a table of rentals with status and payment badges" width="640">
</p>

Runs a music studio end to end: instrument rentals with QR checkout and returns, recording-room
bookings, invoicing, customer records, and role-based staff accounts. Returns calculate late fees
automatically and send damaged gear to a repair queue; SMS and email reminders go out before and
after each due date; a statistics dashboard exports to PDF. Responsive down to phone width, with
light and dark themes and a <kbd>Ctrl</kbd>+<kbd>K</kbd> command palette.

- **No double-booking under concurrency.** Each booking bumps a per-room lock document inside a
  MongoDB transaction, so two simultaneous bookings for the same room conflict and serialise
  instead of both succeeding.
- **Idempotent reminders.** Each reminder sent is recorded on the rental, so re-running the
  scheduled job never sends the same SMS twice.
- **Server-side money.** Prices, late fees, and invoice totals are always computed on the
  server, never taken from the browser.
- **Session security.** JWT in an `httpOnly`, `SameSite=Strict` cookie with instant revocation,
  a CSRF header check, Zod validation on every request, and rejection of MongoDB operator
  injection.

A deliberate counterpart to the project above: Node and a document database rather than .NET
and a relational one, on the same end-to-end responsibilities.

> **Backend** · Node.js 24 · Express 5 · TypeScript · MongoDB Atlas / Mongoose (multi-document
> transactions) · Zod · AWS SES + SNS · node-cron · Pino · 45 integration tests (Vitest +
> Supertest on an in-memory replica set) · Render
>
> **Frontend** · React 19 · Tailwind CSS v4 · Radix UI · TanStack Query & Table · React Hook
> Form · Recharts · html5-qrcode · jsPDF · Vercel

---

## Engineering impact

Most of my production work lives in private repositories. What it amounted to:

**Performance & cost**
- Brought peak-hour database CPU down from **100% saturation to 75–80%** with targeted B-tree
  indexing, query-plan optimisation, legacy T-SQL refactors, and layered caching — ending
  recurring service degradation under high traffic.
- Cut non-production cloud spend by **~20%** by automating EC2/RDS start-stop schedules with
  AWS Lambda.
- Held **99.9% uptime** through traffic surges using EC2 Auto Scaling groups with custom
  health checks.

**Architecture**
- Replaced a costly, error-prone third-party cron scheduler with a containerised .NET service
  on **ECS Fargate + EventBridge**, making execution idempotent via **DynamoDB conditional
  writes and TTL** so concurrently scheduled containers never double-run a batch.
- Built the ingestion tier for an AI WhatsApp gateway as a **.NET 8 Lambda behind API
  Gateway**, verifying Meta callbacks with constant-time **HMAC-SHA256** and flattening seven
  nested message types into slim SQS job contracts — decoupling AI processing from Meta's
  acknowledgement window.
- Modelled a DynamoDB conversation store around its access patterns: composite partition keys,
  GSIs collapsing per-message lookup to a single bounded query, ISO-8601 sort keys for lexical
  chronology, and atomic `ADD` counters replacing read-modify-write.
- Placed **OpenAI and Anthropic behind segregated interfaces** and typed `HttpClient`s, so
  conversations route per record by configuration rather than by branching.

**Security & delivery**
- Mitigated IDOR vulnerabilities, enforced anti-forgery tokens, and added API rate limiting to
  shut down credential stuffing, automated spam, and unauthorised email relay abuse.
- Integrated the WebXPay payment gateway and built authentication with 2FA, GUID tokenisation,
  and OAuth 2.0 social login.
- Shipped multi-stage Docker builds to ECR/ECS across **two AWS regions** via GitHub Actions,
  with secrets centralised in SSM Parameter Store and EC2 instance-role credentials removing
  deployed secrets entirely.
- Engineered SSR for SEO-critical pages and launched Next.js sub-sites backed by .NET Core APIs.
- Lead code reviews, sign off on QA test cases, mentor engineering interns from onboarding
  through feature delivery, and maintain the team's technical architecture and SOP repository.

---

## Tech stack

| Area | Technologies |
| :--- | :--- |
| **Backend** | C#, .NET 8 / .NET Core, ASP.NET Core (Web API, MVC), Entity Framework Core, Node.js, Express, Python, T-SQL |
| **Frontend** | TypeScript, JavaScript, React, Next.js (SSR), Redux Toolkit, TanStack Query, Tailwind CSS, Vite |
| **Cloud & serverless** | AWS — Lambda, ECS Fargate, EventBridge, SQS, SNS, SES, API Gateway, EC2 Auto Scaling, S3, ECR, VPC, Route 53, ELB, WAF, IAM, Parameter Store, CloudWatch · Azure core & app services · Vercel, Render |
| **Data & persistence** | SQL Server, PostgreSQL, DynamoDB (single-table design, GSIs, conditional writes, TTL), MongoDB, Amazon RDS, T-SQL performance tuning, layered caching |
| **AI & third-party APIs** | OpenAI API (function calling, multimodal extraction), Anthropic Claude API, OpenRouter, prompt engineering, Meta Graph API, Stripe, WebXPay |
| **Testing** | xUnit, Vitest, React Testing Library, Supertest — integration tests against real databases (SQLite, in-memory MongoDB replica sets) rather than mocks |
| **DevOps & practices** | Docker, GitHub Actions CI/CD (multi-region), Git, Jira, Confluence, code review, QA sign-off, mentoring |

---

## Education & certification

- **AWS Certified Solutions Architect – Associate** · SAA-C03 · Amazon Web Services
- **BSc (Hons) in Information Technology** · Sri Lanka Institute of Information Technology (SLIIT) · 2020–2024

---

## Get in touch

- 📧 [pawanlaksahan10@gmail.com](mailto:pawanlaksahan10@gmail.com)
- 💼 [LinkedIn](https://www.linkedin.com/in/pawanlaksahan-22a451209)

*Happy to talk system design, cloud migrations, and putting LLMs into production.*
