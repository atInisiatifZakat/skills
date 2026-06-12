# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a **Claude Code skills repository** — a collection of reusable skill definitions that teach Claude how to conduct structured, interactive workflows. Skills can cover any domain: document generation, code review, data analysis, guided interviews, and more.

## Skill Structure

Each skill lives in its own directory. The only required file is `SKILL.md`; other directories are added as needed:

```
{skill-name}/
  SKILL.md        # Required. Entry point: frontmatter, workflow logic, behavioral rules
  prompts/        # Optional. Supporting prompts (interview questions, validation rules, etc.)
  templates/      # Optional. Blank output templates
```

### SKILL.md Frontmatter

Every `SKILL.md` must start with:

```yaml
---
name: skill-name
description: >
  When and how to trigger this skill. Include trigger keywords here
  so Claude can match user intent accurately.
compatibility: "Claude.ai web/mobile dan Claude Code"
---
```

## Adding a New Skill

1. Create a new directory: `{skill-name}/`
2. Write `SKILL.md` with the frontmatter above, then define: workflow modes, behavioral rules, and output format
3. Add supporting files in `prompts/` and `templates/` if the skill needs them
4. Register the skill in Claude Code settings so it is discoverable via trigger keywords

## Current Skills

| Skill | Domain | Output |
|---|---|---|
| `prd-epic-indonesia` | Product documentation (Indonesian) | `epic.md` + `arch.md` |
| `prd-fitur-indonesia` | Product documentation (Indonesian) | `prd.md` |
| `writing-plans` | Implementation planning — Go & Laravel | `docs/plans/YYYY-MM-DD-<feature>.md` |
| `writing-plans-react` | Implementation planning — React (Vite + TanStack + shadcn/Mantine) | `docs/plans/YYYY-MM-DD-<feature>.md` + `docs/plans/writing-plans-react/discovery.md` |
