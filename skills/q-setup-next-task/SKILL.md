---
name: q-setup-next-task
description: Generate a custom /next-task skill for your project. Ensures your issue tracker, source control, and CI are configured via /q-which-tools, then writes a tailored skill that picks up the next task, implements it, and opens a PR.
---

# Setup /next-task

You are generating a custom `/next-task` skill for the user's project.

## Required categories

This skill needs the following tools.json categories: **issues**,
**source-control**, **ci**

## Process

1. **Ensure tools are configured.** Check if `.quiddity/tools.json` exists and
   contains the `issues`, `source-control`, and `ci` categories. For any missing
   categories, run `/q-which-tools` with those categories (e.g.,
   `/q-which-tools issues source-control ci`).

2. **Read the configuration.** Load `.quiddity/tools.json` and extract the
   `issues`, `source-control`, and `ci` categories. Also read
   `.quiddity/process.md` if it exists for context on branching strategy,
   code review process, and other workflow conventions.

3. **Discover project context.** Read the project's CLAUDE.md, package.json,
   or equivalent to understand what kind of project this is.

4. **Ask skill-specific questions** not covered by tools.json:
   - How should the next task be selected? (highest priority, oldest, specific
     label, assigned to me, etc.)
   - Should the skill assign the issue to the current user?
   - Any PR title/description conventions beyond what's in tools.json?
   - Should it run local checks before pushing?
   - What should it do when blocked? (report back, skip to next task, etc.)

5. **Generate the skill.** Write a complete `/next-task` SKILL.md to the
   project's skills directory. The generated skill should:
   - Query the issue tracker for the highest-priority available task using the
     configured tool and filters
   - Assign the issue and move it to the "In Progress" state
   - Create a branch following the configured naming convention
   - Implement the changes described in the issue
   - Run local CI checks before pushing
   - Open a PR with the right format and link it to the issue
   - Update the issue status
   - Include judgment — if something blocks the task, report back instead of
     plowing ahead
   - Reference `.quiddity/tools.json` for all tool-specific configuration

6. **Show the generated skill** to the user and ask for confirmation before
   finalizing.

## Generated skill template

The generated `/next-task` skill should follow this general structure, adapted
to the user's specific tools:

```markdown
---
name: next-task
description: Pick up the next task, implement it, and open a PR.
---

# /next-task

[Instructions tailored to the user's issue tracker, source control,
branch naming, CI checks, and PR conventions.]
```
