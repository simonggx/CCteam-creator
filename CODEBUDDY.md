# CODEBUDDY.md

This file provides guidance to CodeBuddy Code when working with code in this repository.

## Project Overview

CCteam-creator is a multi-agent team orchestration skill for Claude Code. It sets up parallel AI agent teams with file-based planning, progress tracking, and role-based collaboration. The project provides both English and Chinese variants.

## Project Structure

```
CCteam-creator/
  .claude-plugin/
    marketplace.json              -- Marketplace catalog (lists both EN/CN plugins)
    plugin.json                   -- English plugin metadata
  skills/
    CCteam-creator/               -- English skill
      SKILL.md                    -- Main skill entry point (setup command)
      references/
        roles.md                  -- Role definitions (backend-dev, frontend-dev, researcher, etc.)
        onboarding.md             -- Agent onboarding prompt templates
        templates.md              -- Planning file templates (task_plan.md, CLAUDE.md, etc.)
  cn/                             -- Chinese variant
    .claude-plugin/plugin.json    -- Chinese plugin metadata
    skills/
      CCteam-creator/
        SKILL.md                  -- Chinese version of the skill
        references/
          roles.md / onboarding.md / templates.md
  docs/images/                    -- Screenshots for README
  README.md / README_CN.md        -- Documentation
```

## Key Architecture Concepts

### Team-Lead as Control Plane

The main conversation acts as team-lead — not just a task dispatcher, but the control plane owning:
- User alignment and scope control
- Phase gates (research → development → review → E2E → cleanup)
- Project-global files: main `task_plan.md`, `decisions.md`, project `CLAUDE.md`
- Template-level vs project-local change classification

### File-Based State Persistence

All agent progress persists to `.plans/<project>/`:
- `task_plan.md` — Lean navigation map (not an encyclopedia)
- `docs/` — Project knowledge base (architecture.md, api-contracts.md, invariants.md)
- `<agent-name>/` — Per-agent directory with task folders
- Task folders use prefixes: `task-` (devs), `research-` (researcher), `test-` (e2e-tester), `review-` (reviewer)

### Key Protocols

| Protocol | Description |
|----------|-------------|
| 2-Action Rule | After every 2 search operations, update findings.md |
| 3-Strike Escalation | After 3 failures, escalate to team-lead; no silent retries |
| Doc-Code Sync | Devs MUST update docs/ when code changes APIs/architecture |
| Invariant-Driven Review | Recurring bugs → docs/invariants.md → automated tests |
| Phase Health Check | Verify doc freshness, stale tasks, index integrity at phase boundaries |

### Role Models

| Role | Model | Reasoning |
|------|-------|-----------|
| backend-dev, frontend-dev, reviewer | opus | Deep reasoning for business logic, security review |
| researcher, e2e-tester, cleaner | sonnet | Sufficient for search, testing, pattern-based operations |

## When Modifying This Project

### Template Changes

Changes affecting role definitions, onboarding prompts, CLAUDE.md structure, or dispatch protocols should be made to BOTH:
1. `skills/CCteam-creator/references/` (English)
2. `cn/skills/CCteam-creator/references/` (Chinese)

### SKILL.md Changes

The main skill logic is in `SKILL.md`. Key sections:
- Step 1: Requirements consultation (introduce team, gather requirements)
- Step 2: Confirm the plan (project name, roles, phases)
- Step 3: Create planning files (use templates from templates.md)
- Step 4: Create team + spawn agents (TeamCreate, TaskCreate, spawn in parallel)
- Step 5: Confirm + compact

### Reference File Dependencies

When editing SKILL.md or templates.md, ensure consistency with:
- `roles.md` — Role capabilities, model choices, documentation structure
- `onboarding.md` — Agent onboarding prompts, context recovery rules, task folder structure
- `templates.md` — File templates for task_plan.md, findings.md, progress.md, docs/

## Plugin Metadata

- Version: Defined in `.claude-plugin/plugin.json` and `marketplace.json`
- Marketplace: `jessepwj/CCteam-creator`
- Two plugins: `CCteam-creator` (English) and `CCteam-creator-cn` (Chinese)
