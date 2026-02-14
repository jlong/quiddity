# Quiddity

A set of agent skills that generate custom build-loop skills for any project.

## Project structure

```
skills/
  setup-next-task/SKILL.md   # Generates a /next-task skill
  setup-approve/SKILL.md     # Generates an /approve skill
  setup-new-issue/SKILL.md   # Generates a /new-issue skill
```

## Key concepts

- Each skill is a **setup wizard** — it interviews the user about their systems
  and generates a custom skill in the user's `.claude/skills/` directory.
- Skills should be conversational. Ask questions, don't assume.
- Generated skills should follow the SKILL.md format (YAML frontmatter + markdown).
- The generated skills should reference the user's actual tools (Linear, GitHub,
  Jira, etc.) and conventions (branch naming, commit style, PR format).

## Writing skills

- Keep SKILL.md files under 500 lines. Move reference material to separate files.
- Use YAML frontmatter for metadata (`name`, `description`, `allowed-tools`).
- Support `$ARGUMENTS` for user input when invoking the generated skills.
