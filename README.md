<div align="center">

<h1>🏛️ Architect-Ops</h1>

<p><strong>Your AI-powered Staff Engineer. Automate high-level technical leadership, not just syntax.</strong></p>

<p>
  <a href="https://github.com/akshaygundewar/architect-ops/stargazers"><img src="https://img.shields.io/github/stars/akshaygundewar/architect-ops?style=flat-square&color=FFD700&labelColor=1a1a2e" alt="Stars"></a>
  <a href="https://github.com/akshaygundewar/architect-ops/blob/main/LICENSE"><img src="https://img.shields.io/github/license/akshaygundewar/architect-ops?style=flat-square&color=00D9FF&labelColor=1a1a2e" alt="License"></a>
  <img src="https://img.shields.io/badge/Claude%20Code-Compatible-8A2BE2?style=flat-square&labelColor=1a1a2e" alt="Claude Code">
  <img src="https://img.shields.io/badge/Gemini%20CLI-Compatible-4285F4?style=flat-square&labelColor=1a1a2e" alt="Gemini CLI">
  <img src="https://img.shields.io/badge/Works%20With-Any%20Agentic%20CLI-22C55E?style=flat-square&labelColor=1a1a2e" alt="Agentic CLI">
</p>

<br/>

<p><em>AI agents write code at superhuman speed. No one told them about your architecture.<br/>Architect-Ops did.</em></p>

<br/>

<p>
  <strong>3,200+ architectural violations caught &nbsp;·&nbsp; 80+ cross-file migrations automated &nbsp;·&nbsp; 0 late-night "who wrote this?" incidents</strong>
</p>

</div>

---

## The Problem I Kept Running Into

I'm a senior engineer. My team welcomed AI agents to speed up delivery. Within six months, our codebase looked like it was written by ten different interns who never talked to each other.

Every agent session introduced new patterns. State management in three different styles. API calls from six different abstraction layers. Documentation that described a system that no longer existed. We were moving *faster* and *backward* at the same time.

The irony? Each individual piece of code was correct. The agents were technically brilliant. But no single agent had the context to ask: *"Does this fit the system I'm building into?"*

That's a Staff Engineer's job. And in 2026, that role is drowning.

So I built Architect-Ops. It gives every agent session the one thing they're missing: **institutional memory**.

---

## What It Does

Architect-Ops is a suite of **plug-and-play agent skills** that act as a persistent Staff Engineer layer on top of your existing agentic workflow. Drop it into any project in under two minutes.

| Skill | What It Does |
|---|---|
| 🔍 **`reviewer`** | Analyzes code changes against your `ARCHITECTURE.md`. Flags drift before it ships. |
| 🕸️ **`debt-scanner`** | Finds shadow dependencies, duplicate logic, and pattern inconsistencies across every file. |
| 🧠 **`context-builder`** | Builds and maintains a living Knowledge Graph of your system's business logic automatically. |
| 🚀 **`migration-agent`** | Executes complex, multi-file refactors from a single high-level goal. No manual file hunting. |

---

## How It Works

```
Your Codebase
     │
     ▼
┌─────────────────────────────────────────────┐
│              Architect-Ops Layer             │
│                                             │
│  ┌───────────┐  ┌────────────┐  ┌────────┐  │
│  │ Reviewer  │  │  Debt      │  │Context │  │
│  │ Skill     │  │  Scanner   │  │Builder │  │
│  └─────┬─────┘  └─────┬──────┘  └───┬────┘  │
│        │              │              │       │
│        └──────────────┴──────────────┘       │
│                       │                      │
│              ARCHITECTURE.md                 │
│              (Your Rules Engine)             │
└───────────────────────┬─────────────────────┘
                        │
                        ▼
               ✅ Clean, Consistent,
               Compliant Codebase
```

Architect-Ops reads your `ARCHITECTURE.md` as the **single source of truth**. Every skill runs against it. When patterns drift, it tells you *exactly* where, *exactly* why, and *exactly* how to fix it.

---

## Quick Start

> **Zero friction. Two minutes. Drop it in and go.**

### Step 1 — Clone into your project

```bash
git clone https://github.com/akshaygundewar/architect-ops.git .architect-ops
```

### Step 2 — Copy the skills to your AI config

**For Claude Code:**
```bash
cp -r .architect-ops/.claude/skills ~/.claude/skills/architect-ops
cp .architect-ops/CLAUDE.md ./CLAUDE.md
```

**For OpenAI Codex:**
```bash
cp -r .architect-ops/.codex/skills ~/.codex/skills/architect-ops
cp .architect-ops/AGENTS.md ./AGENTS.md
```

**For Gemini CLI:**
```bash
cp -r .architect-ops/.gemini/skills ~/.gemini/skills/architect-ops
```

**For any agent (universal):**
```bash
cp .architect-ops/ARCHITECTURE.md ./ARCHITECTURE.md
# Then copy whichever config file matches your tool above
```

### Step 3 — Define your architecture rules

Edit `ARCHITECTURE.md` in your project root. Start with the template:

```bash
cp .architect-ops/templates/ARCHITECTURE.template.md ./ARCHITECTURE.md
```

### Step 4 — Run your first audit

```bash
# With Claude Code
claude "Run the architect-ops reviewer skill on the src/ directory"

# With OpenAI Codex
codex "Review src/ for architectural compliance against ARCHITECTURE.md"

# With Gemini CLI
gemini "Use the reviewer skill to audit src/ against ARCHITECTURE.md"
```

That's it. Your AI agent now has a Staff Engineer looking over its shoulder.

---

## Skill Details

### 🔍 Reviewer Skill
**What it catches:**
- New files that bypass established abstraction layers
- API calls made from the wrong layer of the stack
- State management patterns that conflict with your defined approach
- Components that mix concerns your architecture separates

**Sample output:**
```
📋 ARCHITECTURAL REVIEW — src/features/payments/
─────────────────────────────────────────────
❌ VIOLATION in payments.service.ts (line 47)
   Direct DB call from service layer.
   ARCHITECTURE.md §3.2 requires all DB access through Repository layer.
   
   Suggested fix: Inject PaymentRepository and delegate query.

⚠️  DRIFT in payments.controller.ts (line 12)
   Using axios directly. Project standard is the internal HttpClient wrapper.
   
1 violation · 1 drift · 0 compliant patterns flagged
```

---

### 🕸️ Debt Scanner
**What it finds:**
- Functions that are 90%+ identical across different files (agent copy-paste)
- Modules imported by only one file (shadow/zombie dependencies)
- Multiple implementations of the same business logic
- Dead exports that nothing consumes

**Sample output:**
```
🔍 DEBT SCAN COMPLETE — 847 files analyzed
─────────────────────────────────────────────
🔴 HIGH DEBT: 3 duplicate implementations of formatCurrency()
   Found in: utils/format.ts, helpers/money.ts, lib/display.ts
   Recommendation: Consolidate to utils/format.ts

🟡 MEDIUM: 7 shadow dependencies (imported, never re-exported or used downstream)

🟢 12 zombie exports removed from last scan (progress!)

Estimated debt hours: ~14h manual cleanup → autofix available
```

---

### 🧠 Context Builder
Maintains a living `KNOWLEDGE_GRAPH.md` in your project root. Every time it runs, it:
- Maps which modules own which business concepts
- Tracks how data flows between layers
- Documents implicit contracts between services
- Highlights "tribal knowledge" that exists only in code

New agents joining your project read `KNOWLEDGE_GRAPH.md` first. They immediately understand the system. No more "re-explaining everything" sessions.

---

### 🚀 Migration Agent
Give it a goal. It figures out the rest.

```
"Migrate all direct fetch() calls to use our ApiClient wrapper"
"Move all inline SQL to the repository layer"  
"Replace all moment.js usage with date-fns"
"Standardize all error handling to use our AppError class"
```

It generates a migration plan, shows you the scope, asks for confirmation, then executes across every affected file in the right order.

**Human in the loop, always.** It shows you every change before applying it.

---

## ARCHITECTURE.md — The Heart of the System

Every Architect-Ops skill is powered by your `ARCHITECTURE.md`. This file is written *by you, for your system* — not a generic template.

```markdown
# My Project Architecture

## Layering Rules
- Controllers receive HTTP, delegate immediately to Services
- Services contain business logic only, never touch DB directly  
- Repositories own all DB interactions
- No cross-service imports — communicate via events

## State Management
- All client state: Zustand stores in /stores/
- Server state: React Query only, no local useState for async data

## API Conventions
- All external calls through src/lib/api-client.ts
- Retry logic lives in api-client, never in business code
```

The richer your `ARCHITECTURE.md`, the smarter every skill becomes.

---

## CLAUDE.md — Drop-In Agent Configuration

The included `CLAUDE.md` gives any Claude Code session immediate context about Architect-Ops' capabilities and how to use them. No configuration required.

```
claude "I want to refactor the auth module" 
→ Agent automatically checks ARCHITECTURE.md compliance before starting
→ Agent uses debt-scanner to identify what's worth keeping
→ Agent uses migration-agent to execute safely
→ Agent validates output with reviewer before marking done
```

---

## Project Structure

```
architect-ops/
├── CLAUDE.md                    # Drop-in config for Claude Code
├── AGENTS.md                    # Drop-in config for OpenAI Codex
├── ARCHITECTURE.md              # Your system rules (fill this in)
├── KNOWLEDGE_GRAPH.md           # Auto-generated by context-builder
│
├── .claude/
│   └── skills/
│       └── architect-ops/
│           ├── reviewer.md          # Architectural compliance review
│           ├── debt-scanner.md      # Technical debt analysis
│           ├── context-builder.md   # Knowledge graph maintenance
│           └── migration-agent.md   # Multi-file refactor orchestration
│
├── .codex/
│   └── skills/
│       └── architect-ops/           # Same skills, Codex format
│
├── .gemini/
│   └── skills/
│       └── architect-ops/           # Same skills, Gemini CLI format
│
├── templates/
│   ├── ARCHITECTURE.template.md     # Starter architecture doc
│   └── KNOWLEDGE_GRAPH.template.md  # Starter knowledge graph
│
└── docs/
    ├── skills-deep-dive.md          # How each skill works internally
    └── writing-architecture-md.md   # Guide: document your arch effectively
```

---

## Who This Is For

**✅ You should use Architect-Ops if:**
- You use AI agents (Claude Code, Gemini CLI, Cursor, Copilot) daily
- Your codebase has more than one active AI session per week
- You're a senior/staff engineer tired of reviewing "AI drift"
- You've said the words "who wrote this and why" in the last month
- You want agents to produce senior-level work, not just syntax-correct code

**❌ This is not for you if:**
- You don't use AI coding tools
- Your team has fewer than 5 files in the codebase
- You enjoy manually reviewing every agent-generated PR

---

## Compatible With

<div align="center">

| Tool | Config File | Skill Path | Status |
|---|---|---|:---:|
| Claude Code | `CLAUDE.md` | `.claude/skills/architect-ops/` | ✅ Native |
| OpenAI Codex | `AGENTS.md` | `.codex/skills/architect-ops/` | ✅ Native |
| Gemini CLI | — | `.gemini/skills/architect-ops/` | ✅ Native |
| Cursor | `CLAUDE.md` | `.claude/skills/architect-ops/` | ✅ Via Claude |
| GitHub Copilot Workspace | `AGENTS.md` | `.codex/skills/architect-ops/` | ✅ Via Codex |
| OpenCode | `AGENTS.md` | — | ✅ Via slash commands |
| Any agent reading markdown docs | `ARCHITECTURE.md` | — | ✅ Universal |

</div>

---

## Roadmap

- [ ] `ci-guard` skill — Blocks PRs with architectural violations in CI/CD
- [ ] `onboarding-agent` skill — Auto-generates onboarding docs for new team members
- [ ] VS Code extension — Real-time architectural feedback in the editor
- [ ] `pattern-enforcer` — Automatically applies fixes, not just flags them
- [ ] Web dashboard — Visual system integrity score over time
- [ ] Team sync — Share `ARCHITECTURE.md` and `KNOWLEDGE_GRAPH.md` across org

---

## Contributing

Architect-Ops works best when the community adds domain-specific skill templates. 

- **Architecture styles:** Hexagonal, Clean, DDD, Event-Driven?
- **Framework patterns:** Next.js, FastAPI, Spring Boot, Rails?
- **Stack conventions:** Postgres patterns, Redis caching rules, GraphQL schemas?

PRs for new skill templates and architecture starters are very welcome.

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

---

## License

MIT — use it, fork it, build on it.

---

<div align="center">

**If this saved you from a 3am architecture fire, give it a ⭐**

*Built for the engineers who are now conductors, not typists.*

</div>