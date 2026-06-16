# Trae AI Skills

Open-source AI Skills for [Trae IDE](https://trae.ai/) — 4 battle-tested skills for idea validation, quality-gated development, code health checks, and root-cause debugging.

## Skills

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| **intent-check** | Idea validation | Before writing any code — validate if your idea is worth building |
| **dev-guide** | Quality-gated development | When developing features, fixing bugs, or refactoring with built-in quality checks |
| **code-check** | Codebase health check | When you want to scan code quality across 5 dimensions |
| **bug-hunter** | Root cause debugging | When you see bugs, errors, or unexpected behavior — find the real cause before fixing |

## Installation

1. Copy the skill folder to your Trae IDE skills directory:
   ```
   ~/.trae/skills/
   ```

2. Or create a new skill in Trae IDE and paste the content from `SKILL.md`.

## Usage

In Trae IDE chat, type:

- `/intent-check` — Validate your idea
- `/dev-guide` — Start quality-gated development
- `/code-check` — Run a codebase health check
- `/bug-hunter` — Debug with root cause analysis

## Skill Details

### intent-check
6 hard questions to validate your idea before building. Supports Business Mode (for startups/client work) and Builder Mode (for side projects/internal tools).

### dev-guide
Development with mandatory verification gates. Every code change must be read back and verified. No exceptions.

### code-check
Two-phase codebase health check: Phase A scans 5 dimensions (code quality, test health, dependencies, documentation, architecture), Phase B guides repairs one by one.

### bug-hunter
Systematic debugging: reproduce → trace → hypothesize → fix → verify. Never fix symptoms, always find the root cause.

## License

MIT License — use freely in personal and commercial projects.

## Contributing

Issues and pull requests welcome at [GitHub](https://github.com/sinmon1323/trae-ai-skills).
