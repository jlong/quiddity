---
name: q-setup-tools
description: Walk through the tools in .quiddity/tools.json and help install any missing CLIs or MCPs. Run /q-which-tools first if tools.json doesn't exist.
---

# /q-setup-tools

You are helping the user set up the CLIs and MCPs needed by their project's
tools. Your job is to check what's already installed, guide setup for anything
missing, and let the user skip anything they don't want.

## Prerequisites

`.quiddity/tools.json` must exist. If it doesn't, tell the user to run
`/q-which-tools` first and stop.

## Reference data

`tool-registry.json` (in the same directory as this skill) maps tool names to
recommended CLIs and MCPs. Load it at the start.

## Process

1. **Read tools.json.** Collect all unique `tool` values from every category.

2. **Match against the registry.** For each tool name found in tools.json, look
   it up in `tool-registry.json`. Tools not in the registry are fine — just note
   them as "no setup recommendations available" and move on.

3. **Check what's already installed.** For each matched tool:
   - **CLIs**: Run the `verifyCommand` from the registry. If it succeeds, the
     CLI is already installed. If it fails or the command is not found, it needs
     setup.
   - **MCPs**: Check whether the MCP server is already configured. Look in
     `.claude/settings.json`, `.mcp.json`, or `~/.claude.json` for an existing
     entry matching the package name.

4. **Present a summary.** Show the user a table of all tools, what's installed
   vs. what's missing. For tools with only a `notes` field in the registry,
   display the note. For example:

   ```
   Tool             CLI         MCP
   GitHub           gh (ok)     —
   GitHub Actions   (no setup needed — runs in CI)
   Linear           —           Linear MCP (not found)
   ```

5. **Walk through missing items.** For each tool with something missing:
   - Explain what the CLI or MCP does and why it's useful.
   - Offer to install it or let the user skip.
   - **For CLIs**: Run the `installCommand` from the registry. After
     installation, run `verifyCommand` to confirm it worked. If the CLI needs
     authentication (like `gh auth login`), walk the user through it.
   - **For MCPs**: Help the user add the MCP server configuration. Use
     `claude mcp add` with the package name from the registry, or guide them
     to add it manually. Link to the `docsUrl` for reference.

6. **Summarize.** Show what was set up, what was skipped, and any manual steps
   the user still needs to take (e.g., authentication, API keys).

## Guidelines

- Be conversational but efficient. Don't over-explain tools the user already
  knows.
- If everything is already installed, say so and finish quickly.
- Don't force anything. Every installation should be skippable.
- Prefer CLIs over MCPs. If a tool has both a CLI and an MCP, set up the CLI
  first — it's usually sufficient. Only suggest the MCP if the CLI doesn't
  cover the agent's needs or the user wants it.
- If a tool isn't in the registry, that's fine. Just tell the user there are no
  automatic setup recommendations for it.
