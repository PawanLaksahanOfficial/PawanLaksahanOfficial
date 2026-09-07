# Pawan Laksahan

**Full Stack Software Engineer — .NET & AWS**

I build and operate production SaaS systems end to end: REST and event-driven backends in
C#/.NET, React and Next.js frontends, and the AWS infrastructure underneath them. Three-plus
years on a high-traffic recruitment platform have left me equally comfortable tuning a
saturated SQL Server, hardening an authentication flow, or putting an LLM behind a typed
interface in a message pipeline.

🎓 **AWS Certified Solutions Architect – Associate** (SAA-C03)

---

## Featured projects

### 🎬 Nostalgia AI — [**live demo →**](https://nostalgia-ai-frontend.vercel.app)

[`nostalgia-ai-backend`](https://github.com/PawanLaksahanOfficial/nostalgia-ai-backend) ·
[`nostalgia-ai-frontend`](https://github.com/PawanLaksahanOfficial/nostalgia-ai-frontend)

Turns a written memory and an optional photo into a narrated, captioned video. A background
worker runs the whole pipeline — **LLM narrative → speech synthesis → caption timing → FFmpeg
composition → object storage** — while the client polls for progress. Around it sits the
production machinery that makes it a product rather than a demo: subscription billing with
idempotent Stripe webhooks, atomic per-plan quota enforcement, range-enabled media streaming,
and opaque share tokens with expiry and revocation so a recipient needs no account.

The API is organised as four projects with dependencies pointing inward (Domain → Application
→ Infrastructure → API), which keeps the FFmpeg, storage, email, and payment integrations at
the edges and the business rules independent of them.

> **Backend** · .NET 8 · ASP.NET Core · EF Core 9 · PostgreSQL · Stripe · Cloudflare R2 ·
> AWS SES · FFmpeg · Docker · 114 xUnit tests
>
> **Frontend** · React 19 · TypeScript · Vite · Redux Toolkit · React Router · Vitest

### 🎸 Music Studio & Instrument Rental

[`Music-Studio-and-Instruments-Renting-Management-System`](https://github.com/PawanLaksahanOfficial/Music-Studio-and-Instruments-Renting-Management-System)

Rental and booking management for a music studio — inventory, reservations, and customer
notifications. QR-code check-in for instrument handover, generated PDF invoices, transactional
email and SMS, and scheduled jobs for reminders and overdue returns.

A deliberate counterpart to the project above: Node and a document database rather than .NET
and a relational one, on the same end-to-end responsibilities.

> Node.js · Express · TypeScript · MongoDB / Mongoose · JWT · AWS SES + SNS · node-cron ·
> React · html5-qrcode · jsPDF

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
| **Languages & frameworks** | C#, .NET 8 / .NET Core, ASP.NET Core (Web API, MVC), Entity Framework Core, TypeScript, JavaScript, React, Next.js, Node.js, Python, T-SQL |
| **Cloud & serverless** | AWS — Lambda, ECS Fargate, EventBridge, SQS, SNS, SES, API Gateway, EC2 Auto Scaling, S3, ECR, VPC, Route 53, ELB, WAF, IAM, Parameter Store, CloudWatch · Azure core & app services |
| **Data & persistence** | SQL Server, PostgreSQL, DynamoDB (single-table design, GSIs, conditional writes, TTL), MongoDB, Amazon RDS, T-SQL performance tuning, layered caching |
| **AI & third-party APIs** | OpenAI API (function calling, multimodal extraction), Anthropic Claude API, prompt engineering, Meta Graph API, WebXPay |
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
