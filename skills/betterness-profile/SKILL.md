---
name: betterness-profile
version: 1.0.0
description: "Read and update user profile — name, contact, address, demographics."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# profile

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness profile <command> [flags]
```

## Commands

### `profile get`

Retrieve the full user profile.

```bash
betterness profile get --json
```

Returns: `firstName`, `lastName`, `email`, `phone`, `gender`, `birthDate`, `heightCm`, `weight`, `homeAddress`, `city`, `state`, `zipCode`, `country`

### `profile update`

Update profile fields. Only provided fields are changed.

```bash
# Preview changes
betterness profile update --first-name "Jane" --city "Austin" --dry-run --json

# Apply changes
betterness profile update --first-name "Jane" --city "Austin" --json
```

| Option | Description |
|--------|-------------|
| `--first-name <name>` | First name |
| `--last-name <name>` | Last name |
| `--phone <number>` | Phone number without dial code |
| `--phone-dial-code <code>` | Phone dial code (e.g. +1, +44) |
| `--gender <value>` | `MALE`, `FEMALE`, `OTHER`, or `PREF_NOT` |
| `--birth-date <YYYY-MM-DD>` | Date of birth |
| `--address <street>` | Home street address |
| `--city <city>` | City |
| `--state <state>` | State or province |
| `--zip-code <zip>` | ZIP or postal code |
| `--country <code>` | Country (e.g. US, GB) |
| `--dry-run` | Preview changes without applying |

**Agent rule:** Always use `--dry-run` first, then confirm with the user before applying.
