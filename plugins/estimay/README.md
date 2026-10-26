# Estimay for Claude Code

A Claude Code plugin with [Estimay](https://estimay.io)'s MCP server and a skill for running planning poker the right way round: the assistant prepares the room and records the outcome; the team votes.

## What is Estimay.io?

Estimay is a collaborative planning poker app for agile teams.

- Main site: https://estimay.io
- Product overview and developer docs: https://estimay.io/developers
- MCP server endpoint: https://estimay.io/api/mcp
- Assistant connections (manage or disconnect): https://estimay.io/connect/assistant

```
/plugin marketplace add grynyk/estimay-claude
/plugin install estimay@estimay
```

After marketplace approval (official directory), install with:

```
/plugin install estimay@claude-plugins-official
```

Then run `/mcp`, pick **estimay** and allow it in your browser (read only, or read and write). Ask, for example: _"Set up a planning poker room with the top 8 issues of our backlog."_

What it can do, and how sign-in works: [estimay.io/developers](https://estimay.io/developers). Disconnect at [estimay.io/connect/assistant](https://estimay.io/connect/assistant).

## Publishing notes

- Submit the MCP server separately as an MCP connector (not as a plugin bundle). Use: https://estimay.io/api/mcp
- Bump `version` in `plugins/estimay/.claude-plugin/plugin.json` on every release before merge to main.
- For fast marketplace updates, configure the GitHub push webhook during submission; otherwise updates are picked up on the scheduled check.
