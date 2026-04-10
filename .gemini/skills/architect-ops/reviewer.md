---
name: reviewer
description: Analyzes code changes for architectural compliance against ARCHITECTURE.md. Use with Gemini CLI.
---

# Architect-Ops: Reviewer Skill (Gemini CLI)

This skill is identical in behavior to the Claude Code version.
See `.claude/skills/architect-ops/reviewer.md` for the full specification.

## Gemini CLI Usage

```bash
gemini "Use the reviewer skill to audit src/ against ARCHITECTURE.md"
gemini "Review the payments module for architectural compliance"
gemini "Check if the new auth changes follow our architecture rules"
```

The full skill behavior, output format, and rules are documented in
`.claude/skills/architect-ops/reviewer.md`. This file exists to register
the skill with Gemini CLI's skill discovery.
