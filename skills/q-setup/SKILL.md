---
name: q-setup
description: Run the full Quiddity setup flow. Discovers your process and tools, then generates custom /new-issue, /next-task, and /approve skills for your project.
---

# /q-setup

You are running the full Quiddity setup flow for the user's project. This will
discover their process and tools, then generate all three build-loop skills.

## Process

1. **Welcome the user.** Briefly explain what Quiddity does: it will scan their
   project, learn about their development process and tools, then generate three
   custom skills (`/new-issue`, `/next-task`, `/approve`) tailored to their
   project.

2. **Run /q-scan-project** to scan the project structure. This documents key
   files, tech stack, conventions, and project layout in `.quiddity/project.md`.

3. **Run /q-which-process** to discover the user's SDLC. This captures their
   branching strategy, code review process, issue workflow, and other conventions
   in `.quiddity/process.md`. It can reference `project.md` for context.

4. **Run /q-which-tools** with all categories needed across the three skills:
   ```
   /q-which-tools issues source-control ci pr
   ```
   Because `/q-which-process` has already run, `/q-which-tools` can reference
   `.quiddity/process.md` to pre-fill answers and ask smarter questions.

5. **Run /q-setup-new-issue** to generate the `/new-issue` skill.
   Since project.md, process.md, and tools.json are already populated, this
   will skip the interviews and go straight to skill-specific questions.

6. **Run /q-setup-next-task** to generate the `/next-task` skill.

7. **Run /q-setup-approve** to generate the `/approve` skill.

8. **Summarize.** Show the user what was generated:
   - The location of `.quiddity/project.md`, `.quiddity/process.md`, and `.quiddity/tools.json`
   - The three generated skills and their locations
   - A quick reminder of how to use each skill:
     - `/new-issue [description]` — create a new issue
     - `/next-task` — pick up and implement the next task
     - `/approve [pr-number]` — merge an approved PR
   - Suggest they review each generated skill and tweak as needed
   - Remind them to commit the generated skills to source control
