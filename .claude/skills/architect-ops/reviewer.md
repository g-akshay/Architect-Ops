---
name: reviewer
description: Analyzes code changes for architectural compliance against ARCHITECTURE.md. Flags violations and drift before they ship.
---

# Architect-Ops: Reviewer Skill

## Purpose

The Reviewer Skill performs deep architectural compliance analysis. It does not look for bugs. It looks for patterns that break the system's design integrity.

## When To Use

- Before merging any non-trivial feature branch
- After a heavy AI agent session that touched many files
- When onboarding a new agent to an existing codebase
- On a regular cadence (weekly sprint review)

## Activation Phrases

- "review [directory/file] for architectural compliance"
- "audit the [feature] implementation"
- "check if [file] follows our architecture"
- "run reviewer on [path]"

## Execution Steps

### Step 1 — Load Architecture Rules
Read `ARCHITECTURE.md` from the project root. If it does not exist, STOP and ask the user to create one using `templates/ARCHITECTURE.template.md`.

Extract and index rules by category:
- Layering Rules (§1)
- State Management (§2)  
- API Conventions (§3)
- Naming Conventions (§4)
- Error Handling (§5)

### Step 2 — Enumerate Target Files
List all files in the target scope. For each file, note:
- Which architectural layer it belongs to
- What it imports from other layers
- What it exports

### Step 3 — Analyze Each File

For each file, check against every relevant rule in `ARCHITECTURE.md`.

**Layering checks:**
- Does this file import from a layer it should not access?
- Does this file contain logic that belongs in a different layer?
- Are cross-layer communications using the approved channel?

**Pattern checks:**
- Is state managed using the approved pattern?
- Are API calls going through the approved wrapper?
- Are errors thrown as the approved error class?
- Do names follow the documented conventions?

### Step 4 — Classify Findings

**VIOLATION** — Directly contradicts a rule in `ARCHITECTURE.md`. Must be fixed before merge.
**DRIFT** — Introduces a new pattern not covered by `ARCHITECTURE.md`. Needs discussion.
**ADVISORY** — Technically compliant but may cause future problems. Worth noting.

### Step 5 — Generate Report

Output format:

```
📋 ARCHITECTURAL REVIEW — [target path]
Generated: [timestamp]
Rules checked against: ARCHITECTURE.md (last updated: [date])
─────────────────────────────────────────────────────

❌ VIOLATION — [filename] line [N]
   [Description of what the code does]
   Rule violated: ARCHITECTURE.md §[X.X] — "[exact rule text]"
   Suggested fix: [specific, actionable fix]

⚠️  DRIFT — [filename] line [N]
   [Description of new pattern introduced]
   Not covered by: ARCHITECTURE.md §[closest section]
   Recommendation: [discuss / add to ARCHITECTURE.md / align with pattern X]

💡 ADVISORY — [filename] line [N]
   [Description of potential future issue]

─────────────────────────────────────────────────────
Summary: [N] violations · [N] drifts · [N] advisories
Overall: [BLOCKED / NEEDS REVIEW / APPROVED]
```

### Step 6 — Offer Next Steps

After the report:
- If BLOCKED: offer to generate a fix plan for each violation
- If NEEDS REVIEW: offer to draft a note for `ARCHITECTURE.md` to cover the new patterns found
- If APPROVED: offer to update `KNOWLEDGE_GRAPH.md` with any new patterns confirmed

## Important Rules for This Skill

1. Never modify code while in review mode. Report only.
2. Always cite the exact section of `ARCHITECTURE.md` for every finding.
3. If `ARCHITECTURE.md` is ambiguous, note the ambiguity and ask the user to clarify the rule.
4. Do not flag personal style preferences — only documented rules.
5. A violation is only a violation if `ARCHITECTURE.md` explicitly prohibits it.
