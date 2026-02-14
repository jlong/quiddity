---
name: q-setup
description: Run the full Quiddity setup flow. Configures your tools and generates custom /new-issue, /next-task, and /approve skills for your project.
---

# /q-setup

You are running the full Quiddity setup flow for the user's project. This will
configure their tools and generate all three build-loop skills.

## Process

1. **Welcome the user.** Briefly explain what Quiddity does: it will ask about
   their tools and generate three custom skills (`/new-issue`, `/next-task`,
   `/approve`) tailored to their project.

2. **Run /q-which-tools** with all categories needed across the three skills:
   ```
   /q-which-tools issues source-control ci pr
   ```
   This collects all tool configuration up front so the user is only interviewed
   once.

3. **Run /q-setup-new-issue** to generate the `/new-issue` skill.
   Since tools.json is already populated, this will skip the tool interview
   and go straight to skill-specific questions.

4. **Run /q-setup-next-task** to generate the `/next-task` skill.

5. **Run /q-setup-approve** to generate the `/approve` skill.

6. **Summarize.** Show the user what was generated:
   - The location of `.quiddity/tools.json`
   - The three generated skills and their locations
   - A quick reminder of how to use each skill:
     - `/new-issue [description]` — create a new issue
     - `/next-task` — pick up and implement the next task
     - `/approve [pr-number]` — merge an approved PR
   - Suggest they review each generated skill and tweak as needed
   - Remind them to commit the generated skills to source control
