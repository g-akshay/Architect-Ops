# How Architect-Ops Skills Work

A deep dive into the internals of each skill for contributors and power users.

---

## Architecture Overview

```
User's Project
│
├── ARCHITECTURE.md          ← Ground truth for all skills
├── KNOWLEDGE_GRAPH.md       ← Output of context-builder, input for other skills
├── ARCHITECT_REPORT.md      ← Appended by reviewer and debt-scanner
│
└── .claude/skills/
    └── architect-ops/
        ├── reviewer.md          ← Skill: Compliance enforcement
        ├── debt-scanner.md      ← Skill: Debt identification
        ├── context-builder.md   ← Skill: Knowledge graph maintenance
        └── migration-agent.md   ← Skill: Coordinated refactoring
```

All four skills are stateless markdown instruction sets. They don't run independently — they run *inside* your AI agent session, providing a repeatable prompt strategy that consistently produces expert-level engineering outputs.

---

## Skill: Reviewer

### How It Works
1. Loads `ARCHITECTURE.md` and indexes rules by section
2. Reads target files and builds a mental model of their layer, imports, and exports
3. For each rule in ARCHITECTURE.md, checks if any file violates it
4. Classifies findings: VIOLATION (explicit rule break), DRIFT (uncovered pattern), ADVISORY (future risk)
5. Outputs a structured compliance report

### Why It's Effective
Most AI code reviewers look for bugs. Reviewer looks for *design consistency* — the thing that breaks down fastest in multi-agent environments. By anchoring to a human-written `ARCHITECTURE.md`, it avoids hallucinating rules and only flags what you actually care about.

### Limitations
- Only as good as `ARCHITECTURE.md`. Vague rules → vague output.
- Cannot detect runtime behavior violations (e.g., a function that looks like it's in the right layer but does too much at runtime)
- Works best on strongly-typed codebases where layering is visible in imports

---

## Skill: Debt Scanner

### How It Works
1. Builds a file index: all exports, all imports, approximate function bodies
2. Groups similar functions by name pattern + structural similarity
3. Traces the import graph to find nodes with unusual edge patterns
4. Classifies debt by severity (HIGH/MEDIUM/LOW) and estimates cleanup time

### Why It's Effective
AI agents working in isolated sessions will recreate solutions that already exist elsewhere. They're working from limited context windows, not the full codebase. Debt Scanner finds these collision points — not by finding bugs, but by finding *unnecessary multiplicity*.

### Limitations
- Similarity detection is heuristic, not semantic. It flags structurally similar code, which may occasionally include intentionally duplicated code with slight variations.
- Cannot detect duplicate business logic expressed very differently (e.g., same algorithm in very different styles)
- Zombie export detection assumes static analysis only — dynamic imports won't be caught

---

## Skill: Context Builder

### How It Works
1. Reads all source files and maps business concept ownership
2. Traces data flow through the system following import paths and function calls
3. Extracts tribal knowledge from `// NOTE`, `// HACK`, `// TODO`, and other intent-signaling comments
4. Writes / updates `KNOWLEDGE_GRAPH.md` with concise, agent-readable summaries

### Why It's Effective
The biggest quality gap between agent sessions isn't code quality — it's context. An agent that knows what the system does will make better decisions than one reading raw code. `KNOWLEDGE_GRAPH.md` encodes this context in a single file that any agent can read in seconds at the start of a session.

### Limitations
- Cannot infer business meaning from purely technical code — it summarizes what the code does, not why
- Knowledge graph quality requires some manual annotation for complex business rules
- Needs to be re-run after significant changes to stay accurate

---

## Skill: Migration Agent

### How It Works
1. Parses user's goal into structured transformation specification
2. Finds all files matching the "before" pattern using code analysis
3. Generates a migration plan with risk classification per file
4. Waits for explicit user approval
5. Executes changes in dependency order: shared utilities → leaf modules → business logic → tests
6. Runs `/reviewer` on output automatically

### Why It's Effective
Most large-scale refactors fail because they're done incrementally over weeks, leaving the system in a mixed state. Migration Agent plans the full scope upfront, executes atomically (or in approved batches), and validates the result — matching how a senior engineer would actually approach a systemic change.

### The Human-In-The-Loop Requirement
Migration Agent will never execute without explicit "execute" confirmation. This is intentional:
- Large changes are irreversible in most workflows
- The plan phase is valuable — users often discover the scope is different than expected
- Confidence in automated changes comes from understanding the plan first

---

## Contributing New Skills

A skill is a markdown file with:
1. A YAML frontmatter block (`name`, `description`)
2. A Purpose section explaining what the skill is for
3. Execution steps the agent should follow
4. An output format specification
5. Important rules / edge cases

Skills should be:
- **Repeatable**: same input consistently produces the same structure of output
- **Scoped**: one skill, one concern
- **Human-gated at decision points**: skills that make changes always show a plan first
- **Self-contained**: skills should work only with files in the user's project, no external dependencies

See existing skills in `.claude/skills/architect-ops/` for examples.
