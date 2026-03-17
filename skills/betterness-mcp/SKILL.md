---
name: betterness-mcp
version: 1.0.0
description: "Install, uninstall, and manage Betterness MCP server integrations for AI clients (Claude, Cursor, Windsurf)."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# mcp

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness mcp <command> [flags]
```

One-command setup for the Betterness MCP server integration. Writes the correct config to the correct file for each AI client, using stored credentials.

Supported clients: `claude` (Claude Desktop), `claude-code`, `cursor`, `windsurf`.

## Commands

### `mcp install`

Install Betterness MCP into an AI client's config file.

```bash
# Install for Claude Desktop
betterness mcp install claude

# Install for Claude Code (global scope, default)
betterness mcp install claude-code

# Install for Claude Code (project scope — writes .mcp.json in cwd)
betterness mcp install claude-code --scope project

# Install for Cursor
betterness mcp install cursor

# Preview without writing
betterness mcp install claude --dry-run
```

| Option | Description |
|--------|-------------|
| `<client>` | **Required.** Target client: `claude`, `claude-code`, `cursor`, `windsurf` |
| `--scope <scope>` | `global` or `project` (claude-code only, default: `global`) |
| `--dry-run` | Print the config that would be written without making changes |

**What it does:**
1. Resolves credentials (same priority as all CLI commands)
2. Reads existing client config file (if any)
3. Merges `mcpServers.betterness` entry, preserving all other config
4. Backs up original file as `<filename>.bak`
5. Writes updated config

Returns: `client`, `configFile`, `status`, `apiKey` (truncated), `backup`

### `mcp uninstall`

Remove the Betterness entry from a client's MCP config.

```bash
betterness mcp uninstall claude
betterness mcp uninstall claude-code --scope project
```

| Option | Description |
|--------|-------------|
| `<client>` | **Required.** Target client to unconfigure |
| `--scope <scope>` | `global` or `project` (claude-code only, default: `global`) |

Does **not** revoke the API key — use `betterness auth logout` for that.

### `mcp status`

Show which AI clients have Betterness MCP configured.

```bash
betterness mcp status --json
```

Scans all known config file paths and reports:
- Whether each client has a `mcpServers.betterness` entry
- The API key in use (truncated)
- The config file path
- The MCP endpoint URL

## Config File Locations

| Client | macOS | Linux |
|--------|-------|-------|
| Claude Desktop | `~/Library/Application Support/Claude/claude_desktop_config.json` | `~/.config/Claude/claude_desktop_config.json` |
| Claude Code (global) | `~/.claude/settings.json` | `~/.claude/settings.json` |
| Claude Code (project) | `.mcp.json` in project root | `.mcp.json` in project root |
| Cursor | `.cursor/mcp.json` in project root | `.cursor/mcp.json` in project root |
| Windsurf | `~/.codeium/windsurf/mcp_config.json` | `~/.codeium/windsurf/mcp_config.json` |

## Tips

- Run `mcp install` after `auth login` — it reuses the same credentials.
- After installing, **restart the AI client** for the config to take effect.
- Use `--dry-run` to inspect the config before writing, especially if the client already has other MCP servers configured.
- Use `mcp status --json` to programmatically check integration state across all clients.
- The install is idempotent — running it again updates the entry (useful after rotating API keys).
