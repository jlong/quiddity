---
name: q-scan-project
description: Scan the current project and create a summary of its structure, important files, and conventions in .quiddity/project.md.
---

# /q-scan-project

You are scanning the user's project to understand its structure and document
what you find. Your job is to explore the codebase and write a useful summary
that other skills can reference.

## Process

1. **Check for existing project.md.** If `.quiddity/project.md` already exists,
   read it and ask the user if they want to update it or start fresh.

2. **Scan the project.** Look for the following and note what you find:

   ### Key documentation files
   - README, README.md
   - ARCHITECTURE.md, DESIGN.md
   - CONTRIBUTING.md
   - CHANGELOG.md
   - ROADMAP.md
   - CLAUDE.md, .cursorrules, or other agent configuration
   - Any docs/ or documentation/ directory

   ### Project configuration
   - package.json, Cargo.toml, pyproject.toml, go.mod, Gemfile, etc.
   - What language(s) and framework(s) does the project use?
   - What package manager? (npm, pnpm, yarn, bun, cargo, pip, etc.)
   - Monorepo structure? (workspaces, lerna, turborepo, etc.)

   ### Source structure
   - Where does source code live? (src/, lib/, app/, etc.)
   - Entry points (main files, index files, etc.)
   - Test directory structure (tests/, __tests__/, spec/, etc.)

   ### CI/CD and configuration
   - .github/workflows/, .circleci/, Jenkinsfile, etc.
   - Docker files (Dockerfile, docker-compose.yml)
   - Deployment configuration (vercel.json, fly.toml, etc.)

   ### Existing conventions
   - .editorconfig, .prettierrc, .eslintrc, etc.
   - PR templates (.github/pull_request_template.md)
   - Issue templates (.github/ISSUE_TEMPLATE/)
   - CODEOWNERS

   ### Plans and specs
   - Any planning documents, RFCs, ADRs (architecture decision records)
   - Spec files or design documents
   - TODO files

3. **Ask clarifying questions.** If anything is ambiguous, ask the user:
   - Is there anything important about the project structure not captured above?
   - Are there any conventions or patterns that aren't obvious from the files?

4. **Write the results.** Create `.quiddity/project.md` with a clear summary.
   Create the `.quiddity/` directory if it doesn't exist. Write in second person
   ("Your project uses...") so other skills can reference it as context.

5. **Confirm with the user.** Show them the generated project.md and ask if
   anything needs to be adjusted.

## Output format

`.quiddity/project.md` should follow this general structure:

```markdown
# Project Summary

## Overview
[Brief description of what the project is, based on README or package metadata]

## Tech stack
- Language: [e.g., TypeScript]
- Framework: [e.g., Next.js]
- Package manager: [e.g., pnpm]

## Project structure
[Key directories and their purposes]

## Key files
| File | Purpose |
|---|---|
| README.md | Project overview and setup instructions |
| ARCHITECTURE.md | System design and architecture decisions |
| ... | ... |

## Configuration
[Linting, formatting, editor config, etc.]

## CI/CD
[What CI runs, where it's configured]

## Conventions
[Any patterns or conventions discovered]
```

Not all sections need to be present — only include what's relevant.
