---
name: debt-scanner
description: Scans the codebase for agent-induced technical debt — duplicate logic, shadow dependencies, zombie exports, and pattern inconsistencies.
---

# Architect-Ops: Debt Scanner Skill

## Purpose

AI agents working in isolated sessions create a specific kind of technical debt: parallel implementations of the same concept, each locally correct but collectively redundant or inconsistent. The Debt Scanner finds this.

## When To Use

- After a sprint with heavy multi-agent activity
- When the codebase "feels bloated" but you can't pinpoint why
- Before a major architecture refactor (know what you're cleaning before you clean it)
- Monthly as preventive maintenance

## Activation Phrases

- "scan for technical debt in [directory]"
- "find duplicate logic in [path]"
- "identify shadow dependencies"
- "what's redundant in [module]?"
- "tech debt report for [path]"

## Debt Categories to Scan For

### 🔴 HIGH — Blocks Feature Velocity

**Duplicate Business Logic**
- Functions with >70% structural similarity that implement the same business concept
- Same data transformation logic repeated across multiple files
- Multiple error handling patterns doing the same normalization

**Competing Abstractions**
- Two or more utility modules that solve the same problem (e.g., two date formatting utilities)
- Multiple HTTP client wrappers in the same codebase
- Parallel state management approaches for the same data domain

### 🟡 MEDIUM — Slows Development Confidence

**Shadow Dependencies**
- Modules imported into only one other file and never re-exported
- Third-party packages used in only one file that could be abstracted
- Configuration values duplicated across environment files and constants files

**Zombie Exports**
- Functions or classes exported but never imported anywhere
- Type definitions declared but never referenced
- Constants defined but only used with their literal value elsewhere

### 🟢 LOW — Cosmetic / Future Risk

**Inconsistent Naming**
- Files doing the same thing named differently across modules
- Functions following inconsistent verb tenses (fetchUser, getOrder, loadProduct)
- Directory structure inconsistency across features

**Abandoned Patterns**
- Old abstraction that was partially migrated to a new pattern
- Comments referencing removed systems or deprecated APIs
- `TODO` / `FIXME` older than [threshold—default 90 days]

## Execution Steps

### Step 1 — Build File Index
Enumerate all source files. For each file, extract:
- All exported functions/classes/constants
- All imports (internal and external)
- Function signatures and approximate line counts

### Step 2 — Run Similarity Analysis
Group functions with similar names, signatures, and bodies. Flag any group where:
- Names suggest the same concept (format, parse, validate + same noun)
- Logic structure is >70% similar
- Return types are identical or structurally equivalent

### Step 3 — Trace Import Graph
Build an import graph. Flag:
- Nodes with only one inbound edge and no outbound re-exports (shadow dependencies)
- Nodes with exports but zero inbound edges (zombie exports)
- Nodes that are leaves in the graph but are third-party wrappers (abstraction candidates)

### Step 4 — Check Architectural Alignment
If `ARCHITECTURE.md` exists:
- Verify each abstraction category has one, and only one, canonical implementation
- Flag any module that doesn't fit the described layer structure

### Step 5 — Generate Report

```
🔍 DEBT SCAN COMPLETE — [target path]
Generated: [timestamp]
Files analyzed: [N]
─────────────────────────────────────────────────────

🔴 HIGH DEBT

[#] · Duplicate implementations of [concept]
    Found in: [file1, file2, file3]
    Similarity: [N]%
    Canonical candidate: [recommended file to keep]
    Estimated cleanup: [N] hours
    → Autofix available via /migration-agent

[#] · Competing abstractions: [name]
    [description]
    Impact: [what breaks if both remain]

─────────────────────────────────────────────────────
🟡 MEDIUM DEBT

[#] · Shadow dependency: [module name]
    Used only by: [file]
    Recommendation: [inline or promote to shared/]

[#] · [N] zombie exports in [file]
    [list of exports]

─────────────────────────────────────────────────────
🟢 LOW DEBT

[#] · Inconsistent naming pattern in [path]
    [description]

─────────────────────────────────────────────────────
Summary:
  HIGH:    [N] findings · ~[N]h cleanup
  MEDIUM:  [N] findings · ~[N]h cleanup
  LOW:     [N] findings · ~[N]h cleanup
  Total estimated debt: ~[N] engineer-hours

Next steps:
  • Generate fix plan for HIGH items?
  • Run /migration-agent to consolidate duplicate logic?
  • Add debt tracking to ARCHITECT_REPORT.md?
```

## Important Rules for This Skill

1. Report everything before fixing anything. This is a scan, not a refactor.
2. Never delete "zombie" exports without confirming they aren't used in external packages or dynamic imports.
3. Similarity analysis is approximate. Always show examples for human judgment.
4. Estimate cleanup hours conservatively — account for testing and validation time.
