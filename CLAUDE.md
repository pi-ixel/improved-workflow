# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **Claude Code skills repository** — a collection of reusable skill definitions that can be loaded into Claude Code sessions. Each skill lives under `skills/<skill-name>/`.

## Skill Structure

Every skill follows a standard directory layout:

```
skills/<skill-name>/
├── SKILL.md          # Skill definition: frontmatter (name, description) + workflow, quality gates, output rules
├── agents/           # Agent interface configurations (YAML) for UI integration
│   └── <agent>.yaml  # display_name, short_description, default_prompt
└── references/       # Templates and reference documents used by the skill
```

### Key Files

- **`SKILL.md`** — The core skill definition. Contains YAML frontmatter with `name` and `description`, followed by Markdown sections defining the workflow, quality gates, output rules, and template contract.
- **`agents/<agent>.yaml`** — Interface configuration for the skill's UI entry point. Defines `display_name`, `short_description`, and `default_prompt`.
- **`references/`** — Static reference materials (e.g., templates) that the skill reads at runtime. Skills must reference these from their own `<skill-dir>/references/` path, not from project root.

## Current Skills

### us-design-writer

Two-phase skill: first generates a pre-development detailed design document (US/需求详细设计), then after user approval, produces a task breakdown file with independently verifiable tasks.

**Phase 1 — Design document:** Reads template at `references/us设计.md`, outputs `./<需求ID>/<变更描述>-us设计.md`.
**Approval gate:** User can confirm, request modifications (multi-round), or abort.
**Phase 2 — Task breakdown:** Reads confirmed design doc, uses template at `references/任务分解.md`, outputs `./<需求ID>/<变更描述>-任务分解.md`. Tasks use T1/T2 numbering with dependency fields and DoD checklists (function/test/docs).

- **Template contract**: Unknowns must be marked as `【存疑】` with explicit questions — no guessing allowed.
- **Quality gates**: Separate self-checks for each phase (see SKILL.md).
- **Default output**: Each requirement gets its own directory named by 需求ID. design doc → `./<需求ID>/<变更描述>-us设计.md`, task breakdown → `./<需求ID>/<变更描述>-任务分解.md`.

## Conventions

- Skills are self-contained: each skill directory includes everything it needs (templates, agents config).
- Skill descriptions in frontmatter serve as triggers — Claude Code matches user intent against these descriptions to invoke the correct skill.
- The `agents/` YAML files define the interactive entry point; `SKILL.md` defines the full workflow logic.
- Reference templates are fixed artifacts — skills must not modify them at runtime.
