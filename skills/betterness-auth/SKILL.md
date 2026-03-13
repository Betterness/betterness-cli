---
name: betterness-auth
version: 1.0.0
description: "Manage authentication — login, logout, and verify identity."
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

Save API key for authentication. Verifies the key by calling the user profile endpoint.

```bash
# Interactive (prompts for key)
betterness auth login

# Explicit key
betterness auth login --key sk-abc123

# Custom API URL
betterness auth login --key sk-abc123 --url https://staging.betterness.com
```

| Option | Description |
|--------|-------------|
| `--key <apiKey>` | API key to save (prompts if omitted) |
| `--url <apiUrl>` | Backend API URL |

### `auth logout`

Remove stored credentials from `~/.betterness/credentials.json`.

```bash
betterness auth logout
```

### `auth whoami`

Show the currently authenticated user.

```bash
betterness auth whoami --json
```

Returns: `name`, `email`, `id`
