---
name: betterness-connected-devices
version: 1.0.0
description: "Manage health device integrations — list, connect, and disconnect wearables."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# connected-devices

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness connected-devices <command> [flags]
```

## Commands

### `connected-devices list`

List all currently connected health devices and wearables.

```bash
betterness connected-devices list --json
```

Returns: `provider`, `type` (native/cloud), `syncEnabled`, `deviceId`

### `connected-devices available`

List health device integrations the user can connect (not currently active).

```bash
betterness connected-devices available --json
```

Returns: `key`, `name`, `description`

### `connected-devices link`

Generate a connection link for a web-based health device integration.

```bash
betterness connected-devices link --integration-key GARMIN --json
```

| Option | Description |
|--------|-------------|
| `--integration-key <key>` | **Required.** Provider: `GARMIN`, `OURA`, `WITHINGS`, `PELOTON`, `WAHOO`, `EIGHT_SLEEP` |

Returns: a URL the user must visit to authorize the connection.

### `connected-devices apple-health-code`

Generate a connection code for Apple HealthKit via the Junction app.

```bash
betterness connected-devices apple-health-code --json
```

Returns: `connectionCode`

### `connected-devices disconnect`

Disconnect a health device integration.

```bash
# Preview
betterness connected-devices disconnect --integration-key GARMIN --dry-run --json

# Execute
betterness connected-devices disconnect --integration-key GARMIN --json
```

| Option | Description |
|--------|-------------|
| `--integration-key <key>` | **Required.** Provider to disconnect |
| `--dry-run` | Preview the disconnection without executing |

**Agent rule:** Always use `--dry-run` first and confirm with the user before disconnecting.

## Tips

- Run `available` to see which integrations can be connected, then `link` to start the flow.
- A user needs at least one connected device for health data commands (activity, sleep, vitals, body-composition) to return data.
