---
name: q-setup-approve
description: Generate a custom /approve skill for your project. Ensures your source control, CI, and PR conventions are configured via /q-which-tools, then writes a tailored skill that verifies checks and merges approved PRs.
---

# Setup /approve

You are generating a custom `/approve` skill for the user's project.

## Required categories

This skill needs the following tools.json categories: **source-control**,
**ci**, **pr**

## Process

1. **Ensure tools are configured.** Check if `.quiddity/tools.json` exists and
   contains the `source-control`, `ci`, and `pr` categories. For any missing
   categories, run `/q-which-tools` with those categories (e.g.,
   `/q-which-tools source-control ci pr`).

2. **Read the configuration.** Load all available Quiddity context:
   - `.quiddity/project.md` — project structure, CI/CD configuration
   - `.quiddity/process.md` — merge strategy, review requirements,
     post-merge workflow
   - `.quiddity/tools.json` — extract the `source-control`, `ci`, and
     `pr` categories

4. **Ask skill-specific questions** not covered by tools.json:
   - Should the skill accept a PR number as an argument? (e.g., `/approve 123`)
     Or default to the current branch's PR?
   - What should happen after merge? (update issue status, notify, etc.)
   - Should it verify that all review comments are resolved?
   - Any post-merge cleanup steps? (delete local branch, pull latest, etc.)

5. **Generate the skill.** Write a complete `/approve` SKILL.md to the
   project's skills directory. The generated skill should:
   - Accept an optional PR number argument (defaulting to current branch's PR)
   - Verify all CI checks pass
   - Verify no unresolved review comments (if configured)
   - Verify required approvals are met
   - Perform the merge using the configured strategy (squash, merge, rebase)
   - Update the linked issue status (e.g., move to "Done" or "Shipped")
   - Clean up the branch if configured
   - Report clearly if anything blocks the merge
   - Reference `.quiddity/tools.json` for all tool-specific configuration

6. **Show the generated skill** to the user and ask for confirmation before
   finalizing.

## Generated skill template

The generated `/approve` skill should follow this general structure, adapted
to the user's specific tools:

```markdown
---
name: approve
description: Verify checks and merge an approved PR.
argument-hint: "[pr-number]"
---

# /approve

[Instructions tailored to the user's source control, CI checks,
merge strategy, and post-merge workflow.]
```
