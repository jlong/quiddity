---
name: q-which-tools
description: Discover what tools a project uses and save the configuration to .quiddity/tools.json. Pass category names as arguments (e.g., /q-which-tools issues source-control).
argument-hint: "[categories...]"
---

# /q-which-tools

You are interviewing the user about the tools their project uses. Your job is
to ask about each requested category, recommend relevant MCPs, and write the
results to `.quiddity/tools.json`.

## Arguments

`$ARGUMENTS` contains space-separated category names. Supported categories:

- **issues** — Issue/task tracker (Linear, GitHub Issues, Jira, etc.)
- **source-control** — Source control host (GitHub, GitLab, Bitbucket, etc.)
- **ci** — Continuous integration (GitHub Actions, CircleCI, etc.)
- **pr** — Pull request conventions (merge strategy, review requirements, etc.)
- **deploy** — Deployment target (Vercel, AWS, Fly, manual, etc.)
- **notifications** — Notification channels (Slack, Discord, email, etc.)

If no arguments are provided, ask the user which categories they'd like to
configure.

## Process

1. **Read existing config.** Check if `.quiddity/tools.json` exists. If it does,
   read it and note which categories are already configured.

2. **For each requested category:**
   - If the category already exists in tools.json, tell the user it's already
     configured and show the current value. Ask if they want to reconfigure it.
     If not, skip it.
   - If the category is new, ask the user what tool they use.
   - Ask follow-up questions specific to the tool they selected. See the
     category reference below for what to ask.
   - Recommend relevant MCPs and help the user install them if needed.

3. **Write the results.** Merge the new category data into `.quiddity/tools.json`.
   Create the `.quiddity/` directory if it doesn't exist. Preserve any existing
   categories that weren't reconfigured.

## Category reference

### issues

Ask:
- Which issue tracker? (Linear, GitHub Issues, Jira, Shortcut, etc.)
- Team or project name?
- What states/statuses do issues move through? (e.g., Todo → In Progress → Done)
- How is priority represented? (priority field, labels, columns, etc.)
- Any label conventions?

MCP recommendations:
- Linear → suggest Linear MCP
- GitHub Issues → suggest GitHub MCP (may already be available via `gh` CLI)
- Jira → suggest Jira MCP

Example output:
```json
{
  "issues": {
    "tool": "linear",
    "team": "ENG",
    "states": {
      "backlog": "Backlog",
      "todo": "Todo",
      "inProgress": "In Progress",
      "done": "Done",
      "cancelled": "Cancelled"
    },
    "priorities": ["Urgent", "High", "Medium", "Low", "None"],
    "labels": ["bug", "feature", "chore", "improvement"]
  }
}
```

### source-control

Ask:
- Which platform? (GitHub, GitLab, Bitbucket, etc.)
- Default/base branch name? (main, master, develop, etc.)
- Branch naming convention? (e.g., `feat/LIN-{{id}}-{{slug}}`, `feature/JIRA-{{id}}`)
- Commit message format? (conventional commits, free-form, etc.)

MCP recommendations:
- GitHub → suggest GitHub MCP if not already available
- GitLab → suggest GitLab MCP

Example output:
```json
{
  "source-control": {
    "tool": "github",
    "baseBranch": "main",
    "branchFormat": "feat/LIN-{{id}}-{{slug}}",
    "commitFormat": "conventional"
  }
}
```

### ci

Ask:
- Which CI system? (GitHub Actions, CircleCI, Jenkins, etc.)
- What checks run on PRs? (tests, linting, type checking, build, etc.)
- How to run those checks locally? (e.g., `npm test`, `make check`)

Example output:
```json
{
  "ci": {
    "tool": "github-actions",
    "checks": ["tests", "lint", "typecheck", "build"],
    "localChecks": ["npm test", "npm run lint", "npm run typecheck", "npm run build"]
  }
}
```

### pr

Ask:
- Merge strategy? (squash, merge commit, rebase)
- Delete branch after merge?
- Required number of approvals?
- Any PR template in the repo?
- PR title format?

Example output:
```json
{
  "pr": {
    "mergeStrategy": "squash",
    "deleteBranch": true,
    "requiredApprovals": 1,
    "hasTemplate": true,
    "titleFormat": "conventional"
  }
}
```

### deploy

Ask:
- Where does the project deploy? (Vercel, AWS, Fly.io, Heroku, manual, etc.)
- Is deployment automatic on merge to the base branch?
- Any manual steps?

Example output:
```json
{
  "deploy": {
    "tool": "vercel",
    "automatic": true,
    "trigger": "merge-to-main"
  }
}
```

### notifications

Ask:
- Where should notifications go? (Slack, Discord, email, none)
- Which channel/room?
- What events should trigger notifications?

MCP recommendations:
- Slack → suggest Slack MCP

Example output:
```json
{
  "notifications": {
    "tool": "slack",
    "channel": "#engineering",
    "events": ["pr-merged", "deploy-complete"]
  }
}
```
