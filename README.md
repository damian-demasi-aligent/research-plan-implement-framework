# Research Plan Implement Framework

This repository defines a practical **Research -> Plan -> Implement** workflow for AI-assisted software delivery.

It contains:
- command playbooks in `.claude/commands/` for running consistent workflows
- specialist agent guides in `.claude/agents/` for locating, analyzing, and documenting code/docs
- shared Claude settings in `.claude/settings.json` (and local overrides in `.claude/settings.local.json`)

## What this repo is for

Use this project as an operating framework for AI agents so they:
- research the real codebase before making decisions
- create implementation plans with clear scope and verification criteria
- execute plans in controlled phases with explicit validation
- keep outputs aligned to repository conventions (for example `docs/tickets`, `docs/plans`, `docs/research`)

## Core workflow

1. **Research** with `.claude/commands/research_codebase.md` to map current implementation and constraints.
2. **Plan** with `.claude/commands/create_plan.md` to produce a detailed, phased implementation plan.
3. **Implement** with `.claude/commands/implement_plan.md` to deliver the plan incrementally and safely.
4. **Validate/Review** using related command guides (for example `validate_plan.md`, `local_review.md`).

## Why this helps

This framework reduces guesswork and makes AI output more consistent by enforcing:
- evidence-backed analysis (file/path references)
- explicit scope boundaries
- measurable success criteria
- repeatable, team-friendly delivery patterns
