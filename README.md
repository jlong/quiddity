# Quiddity

Generate custom build-loop skills for any project.

Quiddity provides a set of setup skills that walk you through configuring a
complete [build loop](https://32pixels.co/blog/how-i-manage-my-dev-workflow-with-three-agent-skills)
for your project. It asks about your systems, helps you install the right MCPs
and tools, and writes custom `/next-task`, `/approve`, and `/new-issue` skills
tailored to your workflow.

## Install

```bash
npx skills add jlong/quiddity
```

## Skills

| Skill | Description |
|---|---|
| `/setup-next-task` | Generate a `/next-task` skill for your project |
| `/setup-approve` | Generate an `/approve` skill for your project |
| `/setup-new-issue` | Generate a `/new-issue` skill for your project |

Each setup skill will:

1. Ask about your project's tools and systems (issue tracker, CI, git workflow, etc.)
2. Recommend and help install any needed MCPs or CLI tools
3. Generate a custom skill in `.claude/skills/` wired to your specific setup

## What is a build loop?

A build loop is a three-skill workflow that keeps your development cycle moving:

- **`/new-issue`** — Convert plain-English descriptions into structured issues
  in your tracker with proper labels, acceptance criteria, and milestones.
- **`/next-task`** — Pick up the highest-priority task, create a branch,
  implement changes, open a PR, and update the issue status.
- **`/approve`** — Verify checks pass, confirm no unresolved feedback, then
  squash-merge and mark the issue as shipped.

## License

MIT
