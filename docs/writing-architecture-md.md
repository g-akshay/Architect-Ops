# Writing Effective ARCHITECTURE.md Files

This guide helps you write an `ARCHITECTURE.md` that actually makes Architect-Ops skills more powerful.

## Why This Matters

The Reviewer skill is only as strong as your `ARCHITECTURE.md`. Vague rules produce vague feedback. Specific rules produce specific, actionable violations.

---

## The Golden Rule

**Write rules that a capable developer who has never seen your codebase could apply correctly.**

If your rule requires knowing the history of the project to understand, it's tribal knowledge — put it in `KNOWLEDGE_GRAPH.md` instead. `ARCHITECTURE.md` is for _prescriptive_ rules that apply to all future code.

---

## Rule Writing Hierarchy

### Level 1 — Structural Rules (highest enforcement value)
Define what talks to what, and what doesn't.

```markdown
## Layering Rules
- Services never import from other services directly — use events
- Controllers never import from repositories — must go through a service
- Utilities (src/utils/) are pure functions only — no side effects, no imports from business modules
```

These rules can be checked mechanically. The Reviewer skill will catch every violation.

### Level 2 — Pattern Rules
Define *how* things are done, not just *what* they do.

```markdown
## State Management
- Server state: React Query only. No manual fetch in useEffect.
- Optimistic updates: only on mutations where the user's intent is unambiguous
- Global client state: Zustand. One store per business domain.
```

### Level 3 — Convention Rules
Define naming, file placement, and organizational standards.

```markdown
## Naming Conventions
- Feature modules: src/features/[feature-name]/ (kebab-case)
- All exports from a module via index.ts barrel
- Event names: domain:action format (e.g., "user:registered", "order:fulfilled")
```

---

## What to Avoid

### ❌ Vague rules
```markdown
# Bad
- Write clean code
- Keep files small
- Use good patterns
```

### ✅ Specific rules
```markdown
# Good
- No file in src/ may exceed 300 lines. Extract helpers if you're approaching this limit.
- Each feature module exports via a single index.ts barrel — no deep imports from outside the module
```

### ❌ Rules you don't actually enforce
Only put rules in `ARCHITECTURE.md` if you intend to actually enforce them. Every rule is a potential violation the Reviewer will flag. Dead rules create noise and erode trust in the system.

---

## Sections to Always Include

1. **Layering Rules** — The most critical. Without these, agents will put logic anywhere.
2. **State Management** — Prevents the most common source of agent-induced inconsistency.
3. **API Conventions** — Controls how the system talks to the outside world.
4. **Naming Conventions** — Makes the codebase navigable for future agents and humans.

## Sections Worth Adding For Large Codebases

5. **Error Handling** — Prevents 5 different error patterns emerging from 5 different sessions.
6. **Testing Strategy** — Ensures agents write tests that actually match your CI requirements.
7. **Security Constraints** — Non-negotiable rules that should never be overridden.
8. **Performance Rules** — Prevents common N+1, bundle bloat, and cache anti-patterns.

---

## Updating ARCHITECTURE.md Over Time

`ARCHITECTURE.md` should evolve with your system. After any architectural decision:

1. Update the relevant section immediately (while context is fresh)
2. Add an entry to the Change Log
3. Run `/reviewer` on recent code to see if existing code now has violations
4. If violations exist: create a migration plan with `/migration-agent`

### When a Drift Becomes a New Rule

Sometimes the `/reviewer` flags a DRIFT — a pattern not covered by your `ARCHITECTURE.md`. If the team reviews it and agrees it's a good pattern, add it as a rule:

```markdown
## Added: [date]
- [New rule based on drift that was reviewed and approved]
```

This is how `ARCHITECTURE.md` grows with your system naturally.

---

## Example: Minimal but Effective ARCHITECTURE.md

```markdown
# My API — Architecture

## Layering Rules
- Routes → Services → Repositories (strict directionality)
- No business logic in routes (middleware or handlers)
- No DB access outside of src/repositories/

## API Conventions
- All outbound HTTP via src/lib/http.ts
- All inbound requests validated at route level via Zod schema

## Error Handling
- Throw AppError (src/lib/errors.ts) from services
- Routes wrap service calls and convert errors to HTTP status codes
- Never expose raw database or library errors to HTTP response

## Naming
- Route files: [resource].routes.ts
- Service files: [resource].service.ts
- Repository files: [resource].repository.ts
```

This minimal example is enough for the Reviewer skill to catch 80% of common architectural violations.
