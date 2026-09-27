# DevLog AI: The Engineering Career Copilot Built on Google Cloud Run

> *"You spent 3 months building something incredible. Now tell me what you did."*
>
> Every engineer has heard this at some point — and struggled to answer.

---

## The Problem Nobody Talks About: Engineering Amnesia

Engineering teams are remarkably good at shipping software. They are remarkably bad at documenting what they shipped, why it mattered, and what it meant for the business.

This isn't laziness. It's structural. When you're deep in a distributed systems problem at 11pm, the last thing on your mind is writing a polished achievement statement. So it doesn't happen. Weeks of critical work — latency optimizations, concurrency fixes, security hardening — sink silently into the git log. Pull requests get merged and forgotten.

Then comes performance review season. And the engineer — who shipped more in the last quarter than most engineers ship in a year — stares at a blinking cursor.

**We call this engineering amnesia.** And we built DevLog AI to solve it.

---

## What Is DevLog AI?

**DevLog AI** is an enterprise-grade Engineering Career Copilot deployed on **Google Cloud Run**, powered by **Gemini AI**. It transforms raw, 30-second daily work logs into permanent, quantified career capital — automatically.

Built for the **Google Cloud Run AI Challenge** (`dev-tutorial=cloud-run-ai-challenge`), it is a production-hardened system with:

- 🔁 A **4-tier model fallback ladder** for zero-downtime AI resilience
- 🔒 **Owner-bound Zero-Trust** Firestore security with authenticated subcollections
- 🛡️ Full **OWASP Top 10 + LLM threat zone** coverage (98.4% compliance score)
- 🔑 **Zero-hardcoding hygiene** — API keys managed exclusively via Google Cloud Secret Manager

---

## Feature 1: Engineering Logs — 30 Seconds, Lifelong Value

The core workflow is deceptively simple. An engineer fills a brief structured log:

- **What did you build or fix?**
- **What challenges did you face?**
- **What did you learn?**
- **What was the impact?**

The moment you hit **"Save & Analyze"**, the server-side Gemini engine dissects your entry across **7 career dimensions**:

| Dimension | What It Detects |
|---|---|
| **Technical Impact** | System design decisions, performance engineering, code quality |
| **Business Impact** | Cost savings, revenue-enabling work, metric improvements |
| **Leadership Signals** | Mentoring, roadmap ownership, cross-team influence |
| **Ownership Signals** | On-call accountability, documentation, proactive improvements |
| **Problem Solving** | Debugging depth, novel solutions, root cause analysis |
| **Innovation Signals** | New tools, architectural proposals, process inventions |
| **Cross-Team Collaboration** | Stakeholder alignment, API contracts, shared libraries |

Each log becomes one or more **Achievement Records** — tagged by impact level (HIGH / MEDIUM / LOW), confidence score, and the exact evidence extracted from your own words.

---

## Feature 2: Career Impact Dashboard

Aggregates all your achievement records into a live analytics view:

- **Achievement Velocity** — consistency of high-impact signal generation
- **Category Radar** — are you over-indexed on technical delivery and light on leadership?
- **Emerging Skills** — what new technologies are showing up in your logs?
- **Growth Areas** — where Gemini sees gaps in your trajectory
- **Leadership Signals** — automatic detection of mentoring and ownership moments

---

## Feature 3: Standup Generator — One Click

Synthesizes your recent engineering logs into a **Slack-ready standup**:

- ✅ **Yesterday** — what you shipped or progressed
- 🔨 **Today** — what you're working on next
- 🚧 **Blockers** — friction points detected from your logs

One click. No more morning existential dread.

---

## Feature 4: Sprint Summary — Retros Without the Scramble

End-of-sprint generates a **full executive retrospective** from your logs spanning any date range (7d / 14d / 30d / custom):

- Shipped features and architectural decisions
- Blockers and their resolutions
- Risk remediations and technical debt addressed
- Business impact narrative formatted for your engineering director

---

## Feature 5: Ask Work History — Conversational Career Memory

A **personal RAG engine** over your entire engineering history. Ask natural questions:

- *"What projects produced the highest business impact last quarter?"*
- *"Show examples of times I demonstrated technical leadership."*
- *"What blockers recurred more than twice?"*

Answers include **date-stamped, source-cited evidence** from your own logs — not hallucinations.

---

## Feature 6: Weekly Digest — Close the Loop with Your Manager

Compiles your highest-impact work into a **polished executive report**:

1. 📧 **Email directly** to your manager or tech lead
2. 📄 **Download as PDF** for performance review portfolios
3. 📝 **Export as Markdown** for documentation or GitHub

---

## Technical Architecture

### 🔁 4-Tier Model Fallback Ladder

```
gemini-3.6-flash          ← Primary
    ↓ [503 / 429 / timeout]
gemini-3.1-flash-lite     ← High-Availability Fallback
    ↓ [fail]
gemini-flash-latest       ← Dynamic Alias Fallback
    ↓ [fail]
gemini-3.7-flash          ← Deep Reasoning Fallback
```

The ladder catches HTTP `503`, `429`, `500`, `404`, and `403` errors. Recovery is automatic and transparent — under **300ms** with zero user interruption.

You can watch it live in the **Resilient AI Prompt** tab by checking "Simulate 503 Failover": Attempt 1 fails with `503 UNAVAILABLE`, Attempt 2 auto-recovers on `gemini-3.1-flash-lite`.

### 🔒 Owner-Bound Zero-Trust Firestore

```javascript
// No cross-user access is physically possible
match /users/{userId}/engineeringLogs/{logId} {
  allow read, write: if request.auth != null
                    && request.auth.uid == userId;
}
```

### 🔑 Zero-Hardcoding Secrets Hygiene

The Gemini API key never touches the browser or client code. Injected at Cloud Run container startup via **Google Cloud Secret Manager**:

```bash
gcloud run deploy cloud-run-ai-studio \
  --set-secrets="GEMINI_API_KEY=GEMINI_API_KEY:latest" \
  --source .
```

### 🛡️ OWASP LLM — All 5 Threat Zones Enforced

| Zone | Countermeasure | Status |
|---|---|---|
| **Input Surfaces** | `express.json({ limit: '10mb' })` + null-safe destructuring | ✅ ENFORCED |
| **Planning & Reasoning** | Strict delimiter isolation, system instruction role framing | ✅ ENFORCED |
| **Tool Execution** | Parameter whitelisting, server-side proxies only | ✅ ENFORCED |
| **Memory & State** | Owner-bound Firestore paths + recursive `stripUndefined()` | ✅ ENFORCED |
| **Inter-System Comm** | Cloud Secret Manager + server-side `@google/genai` encapsulation | ✅ ENFORCED |

---

## The Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 19, TypeScript, Tailwind CSS v4, Lucide Icons, Motion |
| **Backend** | Node.js + Express on Google Cloud Run |
| **AI Engine** | `@google/genai` SDK (server-side only) |
| **Database** | Google Cloud Firestore (owner-bound subcollections) |
| **Secrets** | Google Cloud Secret Manager |
| **Auth** | Firebase Authentication (Google Sign-In + Anonymous) |
| **Email Dispatch** | Nodemailer + SMTP |
| **Deployment** | `gcloud run deploy --source .` |
| **Challenge Label** | `dev-tutorial=cloud-run-ai-challenge` |

---

## Why This Matters

Every late-night debugging session, every clever optimization, every time you stepped up when no one asked — it should be counted.

DevLog AI makes the invisible visible. It turns fleeting daily work into durable career evidence. It ensures your professional contributions — real ones, backed by data — are never lost to engineering amnesia.

---

## Demo & Resources

- 📖 [Technical README](./README.md) — Setup, deployment, security configuration
- 🌐 **Live Demo**: Deployed on Google Cloud Run
- 🏗️ **Built for**: Google Cloud Run AI Challenge — APAC Cohort 3

---

*If you're a builder who's ever lost credit for work you know you shipped — this one's for you.*

---

**Tags:** `#GoogleCloud` `#CloudRun` `#GeminiAI` `#BuildersChallenge` `#EngineeringCareer` `#APAC` `#DevLog` `#AIEngineering` `#OWASP`
