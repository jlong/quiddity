# Architecture

## Overview

Quiddity is a set of agent skills that generate custom build-loop skills for
any project. The skills are designed to work together but can each be run
independently.

## Core concepts

### tools.json

`.quiddity/tools.json` is the central configuration file. It stores what tools
a project uses, organized by category:

```json
{
  "issues": {
    "tool": "linear",
    "team": "ENG",
    "states": { "todo": "Todo", "inProgress": "In Progress", "done": "Done" }
  },
  "source-control": {
    "tool": "github",
    "baseBranch": "main",
    "branchFormat": "feat/LIN-{{id}}-{{slug}}"
  },
  "ci": {
    "tool": "github-actions",
    "localChecks": ["npm test", "npm run lint"]
  },
  "pr": {
    "mergeStrategy": "squash",
    "deleteBranch": true
  }
}
```

Each top-level key is a **category**. The schema for each category depends on
the tool selected. `/q-which-tools` is responsible for populating this file.

### Skill dependency flow

```
/q-setup
  │
  ├─ /q-which-tools issues source-control ci pr
  │     └─ writes .quiddity/tools.json
  │
  ├─ /q-setup-new-issue
  │     ├─ calls /q-which-tools issues  (skipped if already configured)
  │     └─ writes .claude/skills/new-issue/SKILL.md (or agent equivalent)
  │
  ├─ /q-setup-next-task
  │     ├─ calls /q-which-tools issues source-control ci
  │     └─ writes .claude/skills/next-task/SKILL.md
  │
  └─ /q-setup-approve
        ├─ calls /q-which-tools source-control ci pr
        └─ writes .claude/skills/approve/SKILL.md
```

### How /q-which-tools works

1. Accepts category names as arguments (e.g., `/q-which-tools issues ci`)
2. Reads `.quiddity/tools.json` if it exists
3. For each requested category:
   - If the category already exists in tools.json, skip it
   - Otherwise, ask the user what tool they use for that category
   - Ask follow-up questions specific to the selected tool
   - Recommend and help install relevant MCPs
4. Merges new results into `.quiddity/tools.json`

### How setup skills work

Each setup skill (`/q-setup-next-task`, `/q-setup-approve`, `/q-setup-new-issue`)
follows the same pattern:

1. Call `/q-which-tools` with the categories it needs
2. Read `.quiddity/tools.json` for the resolved configuration
3. Ask any skill-specific questions not covered by tools.json
   (e.g., commit message format, PR template preferences)
4. Generate a custom SKILL.md tailored to the user's tools and conventions
5. Write the skill to the project's skills directory
6. Show the user the generated skill for confirmation

### How /q-setup works

Orchestrates the full flow:

1. Run `/q-which-tools` with all categories needed across all three skills
2. Run `/q-setup-new-issue`
3. Run `/q-setup-next-task`
4. Run `/q-setup-approve`

Because `/q-which-tools` is called up front with all categories, the individual
setup skills will find everything they need already in tools.json and won't
re-interview.

## File locations

| Path | Purpose |
|---|---|
| `.quiddity/tools.json` | Project tool configuration (generated) |
| `skills/q-setup/SKILL.md` | Full setup orchestrator |
| `skills/q-which-tools/SKILL.md` | Tool discovery and configuration |
| `skills/q-setup-next-task/SKILL.md` | Generates /next-task |
| `skills/q-setup-approve/SKILL.md` | Generates /approve |
| `skills/q-setup-new-issue/SKILL.md` | Generates /new-issue |
