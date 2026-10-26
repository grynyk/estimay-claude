# Estimay for Claude Code

The Claude Code plugin marketplace for [Estimay](https://estimay.io), free planning poker. The `estimay` plugin connects Claude Code to Estimay's MCP server and adds a skill for running planning poker the right way round: the assistant prepares the room and records the outcome; the team votes.

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

Then run `/mcp`, pick **estimay** and allow it in your browser (read only, or read and write).

Not using Claude Code's plugins? Add the server on its own:

```
claude mcp add --transport http estimay https://estimay.io/api/mcp
```

It works in any MCP client that supports remote servers with OAuth (claude.ai custom connectors, Cursor, VS Code, ChatGPT). What it can do and how sign-in works: [estimay.io/developers](https://estimay.io/developers). Disconnect an assistant at [estimay.io/connect/assistant](https://estimay.io/connect/assistant).

Questions or problems: hello@estimay.io.

## Publishing notes

- Submit the MCP server separately as an MCP connector (not as a plugin bundle). Use: https://estimay.io/api/mcp
- Bump `version` in `plugins/estimay/.claude-plugin/plugin.json` on every release before merge to main.
- For fast marketplace updates, configure the GitHub push webhook during submission; otherwise updates are picked up on the scheduled check.

## License

[MIT](LICENSE)
