# Quiddity

Generate custom build-loop skills for any project.

My entire dev workflow runs on [three agent skills](https://32pixels.co/blog/how-i-manage-my-dev-workflow-with-three-agent-skills):
`/new-issue` turns a plain-English description into a well-structured ticket,
`/next-task` picks up the highest-priority item, implements it, and opens a PR,
and `/approve` checks that everything's green, squash-merges, and ships it.

The catch? Every team has its own issue tracker, branch conventions, CI setup,
and review process. A `/next-task` skill that works great with Linear and GitHub
is useless if your team runs Jira and GitLab. These skills have to be bespoke.

Quiddity makes them bespoke for you. It interviews you about your systems,
helps install the right MCPs, and generates `/next-task`, `/approve`, and
`/new-issue` skills tailored to your project. The whole thing grew out of
[building a /next-task skill from scratch](https://32pixels.co/blog/building-a-next-task-agent-skill),
[extracting reusable sub-skills](https://32pixels.co/blog/extracting-sub-skills-from-agent-skills),
and [doing the same for /new-issue](https://32pixels.co/blog/build-a-new-issue-skill-and-stop-writing-issues-by-hand)
— then realizing the setup itself could be automated.

![Quiddity](quiddity-logo.png)

## What is a build loop?

A build loop is a three-skill workflow that keeps your development cycle moving:

- **`/new-issue`** — Convert plain-English descriptions into structured issues
  in your tracker with proper labels, acceptance criteria, and milestones.
- **`/next-task`** — Pick up the highest-priority task, create a branch,
  implement changes, open a PR, and update the issue status.
- **`/approve`** — Verify checks pass, confirm no unresolved feedback, then
  squash-merge and mark the issue as shipped.

## Install

```bash
npx skills add jlong/quiddity
```

## Skills

| Skill                | Description                                         |
| -------------------- | --------------------------------------------------- |
| `/q-setup`           | Run the full setup flow for your project            |
| `/q-scan-project`    | Scan your project and write project.md              |
| `/q-which-process`   | Discover your SDLC and write process.md             |
| `/q-which-tools`     | Interview you about your tools and write tools.json |
| `/q-setup-tools`     | Install missing CLIs and MCPs for your tools        |
| `/q-setup-next-task` | Generate a `/next-task` skill for your project      |
| `/q-setup-approve`   | Generate an `/approve` skill for your project       |
| `/q-setup-new-issue` | Generate a `/new-issue` skill for your project      |

### /q-scan-project

Scans your project and documents its structure, tech stack, key files, and
conventions. Writes `.quiddity/project.md`.

### /q-which-process

Discovers your software development lifecycle. You can share an existing process
document or be interviewed about your branching strategy, code review process,
issue workflow, and other conventions. Writes `.quiddity/process.md`.

### /q-which-tools

Discovers what tools your project uses and writes the results to
`.quiddity/tools.json`. Pass it the categories you need:

```
/q-which-tools issues source-control
```

Reads `process.md` first to pre-fill answers where possible. Categories already
in `tools.json` are skipped unless you ask to reconfigure. Available categories
include: `issues`, `source-control`, `ci`, `pr`, `deploy`, `notifications`,
and others.

### /q-setup-tools

Walks through the tools in `.quiddity/tools.json` and helps install any missing
CLIs or MCPs. Checks what's already set up, offers to install what's missing,
and lets you skip anything you don't want. Run `/q-which-tools` first if
`tools.json` doesn't exist yet.

### /q-setup

Runs the full setup flow in order: `/q-scan-project`, then `/q-which-process`,
then `/q-which-tools`, then `/q-setup-tools`, then each setup skill in sequence.

### /q-setup-next-task, /q-setup-approve, /q-setup-new-issue

Each generates a custom skill for your project. They call `/q-which-tools`
with the categories they need, so they work standalone or as part of `/q-setup`.

## Generated files

Quiddity writes configuration to a `.quiddity/` directory in your project:

| File                   | Purpose                                  |
| ---------------------- | ---------------------------------------- |
| `.quiddity/project.md` | Project structure and tech stack summary |
| `.quiddity/process.md` | Your development process and conventions |
| `.quiddity/tools.json` | Concrete tool configuration by category  |

These files should be committed to source control so your whole team shares the
same configuration. The generated skills (in your agent's skills directory)
should also be committed.

## License

MIT License (See [LICENSE.md](LICENSE.md))
