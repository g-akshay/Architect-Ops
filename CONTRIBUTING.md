# Contributing to Architect-Ops

First off — thank you. Architect-Ops grows most powerful when people contribute domain-specific skills and architecture starters.

## What We Need Most

### 1. Architecture Starters
Real-world `ARCHITECTURE.md` examples for common tech stacks and patterns:

- Framework-specific: Next.js App Router, FastAPI, Spring Boot, Rails, NestJS, Go services
- Pattern-specific: Hexagonal Architecture, Clean Architecture, DDD, Event-Driven, CQRS
- Stack-specific: Supabase + Next.js, Postgres + Prisma, GraphQL monorepo

**To contribute:** Add to `templates/architectures/[framework-or-pattern].md`

### 2. Skill Extensions
New skills that extend the core four for domain-specific use cases:

- `ci-guard`: Blocks PRs with architectural violations in CI
- `dependency-auditor`: Tracks license compliance and security advisories
- `onboarding-doc-generator`: Creates onboarding docs from `KNOWLEDGE_GRAPH.md`
- `test-coverage-reviewer`: Checks if new code has adequate test coverage per architecture rules

**To contribute:** Add to `.claude/skills/architect-ops/[skill-name].md`

### 3. Bug Reports & Edge Cases
If a skill produces incorrect output or missing cases, open an issue with:
- The `ARCHITECTURE.md` rule that wasn't enforced correctly
- The code that should have been flagged (or was wrongly flagged)
- The skill name and activation phrase you used

---

## Skill Contribution Guide

### Structure

Every skill file must have:

```markdown
---
name: skill-name
description: One-line description of what this skill does.
---

# Architect-Ops: [Skill Name] Skill

## Purpose
[Why this skill exists — the specific pain it solves]

## When To Use
[Specific scenarios]

## Activation Phrases
[The phrases that trigger this skill]

## Execution Steps
[Numbered, detailed steps the agent should follow]

## Output Format
[Exact format specification with examples]

## Important Rules for This Skill
[Edge cases, human-gate requirements, limitations]
```

### Quality Bar

Skills must be:
- **Repeatable**: Same input consistently produces the same output structure
- **Scoped**: One skill, one concern. No "do everything" skills.
- **Human-gated**: Skills that modify files must show a plan and wait for approval before executing
- **Honest about limitations**: Document what the skill cannot do

### Testing Your Skill

Before submitting, test your skill by:
1. Creating a sample project with deliberate violations
2. Activating your skill with Claude Code or Gemini CLI
3. Verifying the output matches your specified format
4. Documenting the test case in your PR

---

## Architecture Starter Contribution Guide

An architecture starter is a ready-to-use `ARCHITECTURE.md` for a specific tech stack or pattern.

### Requirements
- Must be based on real-world, widely-adopted conventions — not personal preferences
- Must include all core sections: Layering, State Management, API Conventions, Naming
- Should include a brief intro explaining what stack/pattern this covers
- Link to official documentation where the conventions come from

### Naming
`templates/architectures/[descriptor].md`

Examples:
- `nextjs-app-router.md`
- `nestjs-clean.md`
- `fastapi-hexagonal.md`
- `event-driven-microservices.md`

---

## PR Guidelines

- One skill or one architecture starter per PR
- Include a brief description of the real-world pain point this addresses
- If contributing a skill: include a sample output showing what the skill produces
- If contributing an architecture starter: note which parts are opinionated vs. community consensus

---

## Code of Conduct

Be excellent to each other. This is a tool for senior engineers — bring that same thoughtfulness to how you collaborate here.

---

*Questions? Open an issue or start a discussion.*
