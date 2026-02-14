---
name: q-which-process
description: Discover your project's software development lifecycle (SDLC) and write a process description to .quiddity/process.md. Accepts an existing document or interviews you to understand your workflow.
---

# /q-which-process

You are discovering the user's software development lifecycle (SDLC). Your job
is to understand how they work — not what tools they use (that's `/q-which-tools`),
but the workflow, conventions, and processes that govern their development cycle.

## Process

1. **Check for existing documentation.** Ask the user:
   - "Do you have a document that describes your development process? You can
     share a file path, URL, or paste the contents."
   - If they provide a document, read it and extract the relevant details below.
   - If not, interview them using the questions below.

2. **Check for existing process.md.** If `.quiddity/process.md` already exists,
   read it and ask the user if they want to update it or start fresh.

3. **Interview the user** about the following areas (skip any that were covered
   by a shared document):

   ### Branching strategy
   - What branching model? (trunk-based, git-flow, GitHub flow, etc.)
   - Base/main branch name?
   - Branch naming conventions? (e.g., `feat/`, `fix/`, `chore/` prefixes)
   - Do feature branches live long or are they short-lived?
   - Any release branches?

   ### Code review process
   - Are PRs required for all changes?
   - How many reviewers are required?
   - Who typically reviews? (team lead, any team member, CODEOWNERS, etc.)
   - Any review conventions? (e.g., "approve with comments" vs "request changes")
   - Turnaround time expectations?

   ### Issue workflow
   - How does work get prioritized and assigned?
   - What does the lifecycle of an issue look like? (triage → backlog → sprint → in progress → review → done)
   - Who creates issues? (PMs, engineers, anyone?)
   - How are bugs vs features vs chores distinguished?
   - Sprint/cycle cadence? (weekly, biweekly, continuous, etc.)

   ### Commit and PR conventions
   - Commit message format? (conventional commits, free-form, ticket prefix, etc.)
   - PR title/description conventions?
   - Any PR templates?
   - Squash, merge, or rebase?

   ### Testing expectations
   - Are tests required for all changes?
   - What kinds of tests? (unit, integration, e2e, etc.)
   - Test coverage requirements?
   - Who is responsible for writing tests? (author of the change, QA team, etc.)

   ### Release and deployment
   - How are releases managed? (continuous deployment, scheduled releases, manual)
   - Versioning scheme? (semver, calver, none)
   - Any changelog or release notes process?
   - Staging/preview environments?

   ### Team structure
   - Solo developer or team?
   - If team: how is work divided? (feature teams, full-stack, frontend/backend split)
   - Any on-call or rotation responsibilities?

4. **Write the results.** Create `.quiddity/process.md` with a clear, readable
   description of the user's SDLC. Use markdown headings and bullet points.
   The file should be written in second person ("You use trunk-based development...")
   so that other skills can reference it as instructions.

   Create the `.quiddity/` directory if it doesn't exist.

5. **Confirm with the user.** Show them the generated process.md and ask if
   anything needs to be adjusted.

## Output format

`.quiddity/process.md` should follow this general structure:

```markdown
# Development Process

## Branching strategy
[description]

## Code review
[description]

## Issue workflow
[description]

## Commit and PR conventions
[description]

## Testing
[description]

## Release and deployment
[description]

## Team structure
[description]
```

Not all sections need to be present — only include what's relevant to the
user's project.
