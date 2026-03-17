---
name: betterness-auth
version: 1.1.0
description: "Manage authentication — OAuth login, API key login, logout, and verify identity."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# auth

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for global flags and security rules.

```bash
betterness auth <command> [flags]
```

## Commands

### `auth login`

Authenticate with Betterness. Uses OAuth by default (opens a browser window). Alternatively, use `--key` for API key authentication.

Only one auth method is active at a time — logging in with one clears the other.

```bash
# OAuth (default) — opens browser for secure login
betterness auth login

# API key
betterness auth login --key bk_abc123
```

| Option | Description |
|--------|-------------|
| `--key <apiKey>` | Use an API key instead of OAuth |

**OAuth flow details:**
- Opens a local callback server on port `19847`
- Prints an Auth0 URL to open in the browser
- Waits up to 120 seconds for authentication
- Stores tokens in `~/.betterness/tokens.json` (auto-refreshed when expired)

**API key flow:**
- Verifies the key against the backend
- Stores credentials in `~/.betterness/credentials.json`

### `auth logout`

Remove all stored credentials (both OAuth tokens and API key).

```bash
betterness auth logout
```

### `auth whoami`

Show the currently authenticated user.

```bash
betterness auth whoami --json
```

Returns: `name`, `email`, `id`
