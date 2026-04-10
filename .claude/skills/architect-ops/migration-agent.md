---
name: migration-agent
description: Executes complex, multi-file refactors from a single high-level goal. Plans first, executes with human approval, validates with /reviewer on completion.
---

# Architect-Ops: Migration Agent Skill

## Purpose

Large-scale refactors are dangerous when done incrementally over days. Each partial step leaves the system in an inconsistent state. Migration Agent executes the entire change atomically with a clear plan, explicit approval gates, and built-in validation.

It converts a goal like "standardize all error handling" into a structured, reviewable execution plan — then executes it.

## When To Use

- Consolidating duplicate logic identified by `/debt-scanner`
- Migrating from one library/pattern to another across many files
- Executing changes mandated by an `ARCHITECTURE.md` update
- Any refactor that touches >3 files with related, coordinated changes

## Activation Phrases

- "migrate all [X] to use [Y]"
- "replace [old pattern] with [new pattern] across the codebase"
- "refactor all [X] to follow [convention]"
- "consolidate [concept] into [module]"
- "execute the migration from [state A] to [state B]"

## Execution Steps

### Phase 1 — Goal Parsing

Parse the user's goal into:
- **What is changing**: the old pattern/approach being replaced
- **What it becomes**: the new pattern/approach
- **Why**: the architectural reason (link to `ARCHITECTURE.md` section if applicable)
- **Scope**: which directories/file types are in scope

Example:
```
Goal: "Replace all direct fetch() calls with our ApiClient wrapper"

Parsed:
  Old pattern:  fetch(url, options) called directly in components/services
  New pattern:  apiClient.get(url, options) from src/lib/api-client.ts
  Rule:         ARCHITECTURE.md §3 requires all HTTP through ApiClient
  Scope:        src/ (all .ts, .tsx files)
```

### Phase 2 — Scope Analysis

**Enumerate all affected files.** For each file:
- Location of old pattern (line numbers)
- Context around each instance (what it's doing)
- Estimated change complexity: SIMPLE / MODERATE / COMPLEX
- Dependencies that may need to change (imports, types)

**Risk assessment:**
- Files with many callsites: higher risk
- Files that are test mocks of the old pattern: require separate strategy
- Files that wrap the old pattern (already partial migration): flag carefully

### Phase 3 — Migration Plan

Present the full plan to the user before any changes:

```
📦 MIGRATION PLAN
─────────────────────────────────────────────────────
Goal: [user's goal]
Rule: ARCHITECTURE.md §[X.X]

Scope:
  [N] files affected
  [N] callsites to change
  Estimated time: [N] minutes

Files to change:
  SIMPLE (automated, low risk):
    ✓ [file1] — [N] callsites
    ✓ [file2] — [N] callsites

  MODERATE (review after change):
    ⚠ [file3] — [N] callsites, [reason for caution]

  COMPLEX (step-by-step):
    ⚡ [file4] — [N] callsites, [what makes this complex]

Files NOT in scope (and why):
    ✗ [file5] — [reason: test mock, external package, etc.]

Risk level: [LOW / MEDIUM / HIGH]
Rollback strategy: [git revert / stash / list of manual undos]

New imports required: [list]
Types to update: [list]
Tests to update: [list]

─────────────────────────────────────────────────────
Type "execute" to proceed, or ask questions first.
Type "execute [filename]" to run on a single file first.
```

**STOP HERE. Wait for explicit user approval.**

### Phase 4 — Execution

Execute changes in this order:
1. Shared utilities and types first (lowest risk, others depend on them)
2. Leaf modules (used by others, but don't depend on many things)
3. Business logic modules (services, controllers)
4. Test files last

For each batch:
- Apply the changes
- Show a diff summary
- Confirm before proceeding to the next batch (unless user said "run all")

### Phase 5 — Post-Migration Validation

After all changes are applied:
1. **Run `/reviewer`** on all changed files against `ARCHITECTURE.md`
2. Check for any new violations introduced by migration
3. Report any files where the migration was partial or incomplete
4. Update `KNOWLEDGE_GRAPH.md` to reflect the new pattern as canonical

### Phase 6 — Completion Report

```
✅ MIGRATION COMPLETE
─────────────────────────────────────────────────────
Goal: [original goal]

Results:
  Files changed: [N]
  Callsites migrated: [N]
  Files skipped: [N] ([reason])

Reviewer output:
  Violations introduced: [N] (should be 0)
  Architectural compliance: [PASS / REVIEW NEEDED]

Knowledge Graph: [updated / needs update]

Remaining work (if any):
  • [anything that needs manual follow-up]
```

## Migration Templates

### Library Replacement

```
Old: import [lib] from '[old-package]'
New: import [lib] from '[new-package]'
API changes: [list any method renames or signature changes]
```

### Pattern Consolidation

```
Old: [code pattern, can be multi-line]
New: [replacement code pattern]
Where to import new pattern from: [module path]
```

### Abstraction Layer Insert

```
Before:  [lower level call]
After:   [call via new wrapper]
Wrapper location: [path]
Wrapper interface: [method signatures]
```

## Important Rules for This Skill

1. **Never execute without explicit "execute" confirmation from the user.**
2. **Never combine a migration with unrelated changes.** If you notice something else wrong while migrating, note it separately — do not fix it inline.
3. **Show diffs, don't just apply.** For COMPLEX files, show the diff and confirm before applying.
4. If a callsite's context is unusual or unclear, flag it for human review rather than auto-migrating.
5. If the migration would cause a circular import or break a type contract, STOP and report. Do not proceed.
6. All migrations end with a `/reviewer` pass. Migrations that introduce violations are incomplete.
