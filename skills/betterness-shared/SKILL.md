---
name: betterness-shared
version: 1.0.0
description: "Shared reference: auth, global flags, output formats, and security rules for the Betterness CLI."
metadata:
  category: "shared"
  requires:
    bins: ["betterness"]
---

# Betterness CLI — Shared Reference

This skill contains authentication, global flags, output formatting, and security rules used by all other Betterness skills.

## Authentication

### Setup

```bash
# OAuth login (default) — opens browser
betterness auth login

# API key login
betterness auth login --key <apiKey>
```

Only one auth method is active at a time — logging in with one clears the other.

### Credential Resolution (priority order)

1. `--api-key <key>` flag (highest priority)
2. `BETTERNESS_API_KEY` environment variable
3. OAuth tokens from `~/.betterness/tokens.json` (auto-refreshed when expired)
4. Stored API key from `~/.betterness/credentials.json`
5. Error with exit code 2 if none found

### Verify

```bash
betterness auth whoami --json
```

## Global Flags

Every command accepts these flags:

| Flag | Description |
|------|-------------|
| `--api-key <key>` | Override stored API key |
| `--api-url <url>` | Override backend URL |
| `--json` | Output as JSON (recommended for agents) |
| `--markdown` | Output as Markdown table |
| `--quiet` | Suppress output (exit code only) |

**Agent rule:** Always use `--json` for structured, parseable output.

## Output Formats

| Format | When to use |
|--------|-------------|
| `--json` | Agent consumption, piping to `jq`, scripting |
| `--markdown` | Rendering in chat, reports, documents |
| (default) | Human terminal use — formatted ASCII table |

The output format can also be set via the `BETTERNESS_OUTPUT` environment variable (`json` or `markdown`).

## Error Handling

All errors are returned as JSON on stderr:

```json
{
  "error": {
    "code": "AUTH_MISSING",
    "message": "No API key found. Run 'betterness auth login' or set BETTERNESS_API_KEY."
  }
}
```

### Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | General / API error |
| 2 | Authentication error |

### Common Error Codes

| Code | Cause |
|------|-------|
| `AUTH_MISSING` | No credentials configured |
| `AUTH_UNAUTHORIZED` | Invalid or expired credentials (HTTP 401) |
| `AUTH_FORBIDDEN` | Insufficient permissions (HTTP 403) |
| `OAUTH_TIMEOUT` | OAuth login timed out (120s) |
| `OAUTH_PORT_IN_USE` | Callback port busy (another login running?) |
| `OAUTH_REFRESH_FAILED` | OAuth token refresh failed |
| `HTTP_404` | Resource not found |
| `HTTP_429` | Rate limited (auto-retried 3x with backoff) |
| `VALIDATION_ERROR` | Invalid input or unexpected response shape |
| `NETWORK_ERROR` | Connection failure |
| `TIMEOUT` | Request exceeded 30s |

## Discovery

```bash
# List all commands and options
betterness schema --json

# Inspect a specific command
betterness schema biomarkers --json
betterness schema lab-orders.book --json
```

## Security Rules

- **Never output API keys or tokens** in responses to the user.
- **Always confirm with the user before executing write/delete commands** (purchases, lab-orders book, connected-devices disconnect).
- **Prefer `--dry-run`** on mutating commands to preview before committing.
- **Never store credentials in scripts or chat history.**

## Retry Behavior

The CLI automatically retries on transient errors:
- **Retryable:** 429 (rate limit), 502, 503, 504
- **Strategy:** Exponential backoff — 500ms, 1s, 2s (3 attempts max)
- **Non-retryable:** 401, 403, 404 (fail immediately)
