# Architecture

## Overview

Quiddity is a set of agent skills that generate custom build-loop skills for
any project. The skills are designed to work together but can each be run
independently.

## Core concepts

### process.md

`.quiddity/process.md` describes the user's software development lifecycle in
markdown. It covers branching strategy, code review process, issue workflow,
commit conventions, testing expectations, and release process. Written in second
person so other skills can reference it as instructions.

`/q-which-process` is responsible for generating this file — either by reading
an existing process document the user provides or by interviewing them.

### tools.json

`.quiddity/tools.json` is the tool configuration file. It stores what concrete
tools a project uses, organized by category:

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
It reads `process.md` first to pre-fill answers where possible.

### Skill dependency flow

```
/q-setup
  │
  ├─ /q-which-process
  │     └─ writes .quiddity/process.md
  │
  ├─ /q-which-tools issues source-control ci pr
  │     ├─ reads .quiddity/process.md for context
  │     └─ writes .quiddity/tools.json
  │
  ├─ /q-setup-new-issue
  │     ├─ calls /q-which-tools issues  (skipped if already configured)
  │     └─ writes new-issue/SKILL.md
  │
  ├─ /q-setup-next-task
  │     ├─ calls /q-which-tools issues source-control ci
  │     └─ writes next-task/SKILL.md
  │
  └─ /q-setup-approve
        ├─ calls /q-which-tools source-control ci pr
        └─ writes approve/SKILL.md
```

### How /q-which-process works

1. Asks the user if they have an existing process document to share
2. If yes, reads it and extracts SDLC details
3. If no, interviews them about: branching strategy, code review, issue
   workflow, commit conventions, testing, release process, and team structure
4. Writes `.quiddity/process.md`

### How /q-which-tools works

1. Reads `.quiddity/process.md` for context (if it exists)
2. Accepts category names as arguments (e.g., `/q-which-tools issues ci`)
3. Reads `.quiddity/tools.json` if it exists
4. For each requested category:
   - If the category already exists in tools.json, skip it
   - Otherwise, ask the user what tool they use (pre-filling from process.md)
   - Ask follow-up questions specific to the selected tool
   - Recommend and help install relevant MCPs
5. Merges new results into `.quiddity/tools.json`

### How setup skills work

Each setup skill (`/q-setup-next-task`, `/q-setup-approve`, `/q-setup-new-issue`)
follows the same pattern:

1. Call `/q-which-tools` with the categories it needs
2. Read `.quiddity/process.md` and `.quiddity/tools.json` for context
3. Ask any skill-specific questions not covered by those files
4. Generate a custom SKILL.md tailored to the user's process and tools
5. Write the skill to the project's skills directory
6. Show the user the generated skill for confirmation

### How /q-setup works

Orchestrates the full flow:

1. Run `/q-which-process` to capture the SDLC
2. Run `/q-which-tools` with all categories needed across all three skills
3. Run `/q-setup-new-issue`
4. Run `/q-setup-next-task`
5. Run `/q-setup-approve`

Because `/q-which-process` and `/q-which-tools` are called up front, the
individual setup skills will find everything they need already in process.md
and tools.json and won't re-interview.

## File locations

| Path | Purpose |
|---|---|
| `.quiddity/process.md` | SDLC description (generated) |
| `.quiddity/tools.json` | Tool configuration (generated) |
| `skills/q-setup/SKILL.md` | Full setup orchestrator |
| `skills/q-which-process/SKILL.md` | SDLC discovery and documentation |
| `skills/q-which-tools/SKILL.md` | Tool discovery and configuration |
| `skills/q-setup-next-task/SKILL.md` | Generates /next-task |
| `skills/q-setup-approve/SKILL.md` | Generates /approve |
| `skills/q-setup-new-issue/SKILL.md` | Generates /new-issue |
