# Betterness CLI

**Command-line interface for the Betterness health platform.** Query wearable data, biomarkers, lab results, and health intelligence from your terminal. Ideal for power users, scripts, cron jobs, and AI agent workflows.

---

## Install

```bash
npm install -g @betterness/cli
```

Requires Node.js 18+.

## Authenticate

```bash
betterness auth login          # OAuth in browser
betterness auth whoami         # verify
```

For scripts and CI/CD:

```bash
betterness auth login --key bk_your_api_key
# or
export BETTERNESS_API_KEY=bk_your_api_key
```

## Quick Start

```bash
# Health overview
betterness workflow daily-brief

# Wearable data
betterness activity get --from 2026-03-01 --timezone America/Denver
betterness sleep get --from 2026-03-10
betterness vitals get --from 2026-03-01

# Lab results
betterness biomarkers search --name "Vitamin D"
betterness biomarkers search --range OUT_OF_RANGE

# Connected devices
betterness connected-devices list

# Health profile
betterness health-profile summary

# Knowledge library
betterness knowledge search -s "nutrition"
```

Use `--json` for structured output (recommended for scripts and agents):

```bash
betterness biomarkers search --json | jq '.[] | {name, value, range}'
```

## Commands

| Command | Description |
|---------|-------------|
| `auth` | login, logout, whoami |
| `profile` | get, update |
| `biomarkers` | search, loinc-codes |
| `biological-age` | get |
| `activity` | get (steps, workouts, calories, VO2 max) |
| `sleep` | get, stages |
| `vitals` | get (HR, HRV, BP, SpO2, glucose) |
| `body-composition` | get (weight, body fat, BMI) |
| `connected-devices` | list, available, link, apple-health-code, disconnect |
| `lab-tests` | list (browse available panels) |
| `lab-records` | list, detail |
| `lab-orders` | initialize, service-centers, slots, book, reschedule, cancel |
| `lab-results` | upload, update-status, update-biomarker, update-metadata |
| `purchases` | payment-methods, buy, checkout |
| `smart-listings` | search, detail |
| `workflow` | daily-brief, next-actions |
| `health-profile` | schema, get, get-section, update, reset-section, summary |
| `knowledge` | search |
| `mcp` | install, uninstall, status |
| `schema` | discover commands and formats |
| `debug` | config (show resolved configuration) |

See [CLI_REFERENCE.md](CLI_REFERENCE.md) for the full command reference with all flags and examples.

## MCP Integration

Auto-configure Betterness as an MCP server for your AI client:

```bash
betterness mcp install claude          # Claude Desktop
betterness mcp install claude-code     # Claude Code (global)
betterness mcp install cursor          # Cursor
betterness mcp install windsurf        # Windsurf
betterness mcp status                  # check what's configured
```

## Global Options

| Option | Description |
|--------|-------------|
| `-V, --version` | Print version number |
| `--api-key <key>` | Override stored credentials |
| `--json` | JSON output |
| `--markdown` | Markdown table output |
| `--quiet` | Suppress output (exit code only) |

## Credential Precedence

1. `--api-key` flag (highest)
2. `BETTERNESS_API_KEY` environment variable
3. OAuth tokens from `~/.betterness/tokens.json`
4. Stored API key from `~/.betterness/credentials.json`

## Skills

The [`skills/`](skills/) directory contains modular skill definitions used by AI agents to interact with the CLI:

- **16 service skills** — one per command domain (auth, biomarkers, lab-orders, etc.)
- **2 personas** — health-coach, lab-manager
- **3 recipes** — health-checkup, order-lab-test, track-biomarker

## Links

| Resource | URL |
|----------|-----|
| Website | [betterness.ai](https://betterness.ai) |
| Docs | [betterness.ai/docs](https://betterness.ai/docs) |
| MCP Server | [github.com/Betterness/betterness-mcp](https://github.com/Betterness/betterness-mcp) |
| Builders Portal | [betterness.ai/builders](https://betterness.ai/builders) |
