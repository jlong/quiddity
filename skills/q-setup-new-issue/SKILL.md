---
name: q-setup-new-issue
description: Generate a custom /new-issue skill for your project. Ensures your issue tracker is configured via /q-which-tools, then writes a tailored skill that converts plain-English descriptions into well-structured issues.
---

# Setup /new-issue

You are generating a custom `/new-issue` skill for the user's project.

## Required categories

This skill needs the following tools.json categories: **issues**

## Process

1. **Ensure tools are configured.** Check if `.quiddity/tools.json` exists and
   contains the `issues` category. If not, run `/q-which-tools issues` to
   configure it.

2. **Read the configuration.** Load `.quiddity/tools.json` and extract the
   `issues` category.

3. **Discover project context.** Read the project's CLAUDE.md, package.json,
   or equivalent to understand what kind of project this is.

4. **Ask skill-specific questions** not covered by tools.json:
   - Should the skill accept a description as an argument?
     (e.g., `/new-issue Add dark mode support`)
   - Do issues need acceptance criteria generated automatically?
   - Any standard sections? (description, steps to reproduce, expected behavior)
   - Any issue templates already in use?
   - Auto-assign issues to the creator?

5. **Generate the skill.** Write a complete `/new-issue` SKILL.md to the
   project's skills directory. The generated skill should:
   - Accept a plain-English description (via `$ARGUMENTS` or interactive prompt)
   - Generate a structured issue with proper title, description, and acceptance
     criteria based on the user's conventions
   - Apply the right labels, priority, and milestone using the configured tracker
   - Create the issue via MCP or CLI based on the tool in tools.json
   - Return the issue identifier and URL
   - Reference `.quiddity/tools.json` for all tool-specific configuration

6. **Show the generated skill** to the user and ask for confirmation before
   finalizing.

## Generated skill template

The generated `/new-issue` skill should follow this general structure, adapted
to the user's specific tools:

```markdown
---
name: new-issue
description: Create a new issue from a plain-English description.
argument-hint: "[description]"
---

# /new-issue

[Instructions tailored to the user's issue tracker, labels, states, etc.]
```
