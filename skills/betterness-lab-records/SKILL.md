---
name: betterness-lab-records
version: 1.0.0
description: "View lab records — uploaded results and purchased test orders."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# lab-records

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness lab-records <command> [flags]
```

## Commands

### `lab-records list`

List lab records (both uploaded results and lab orders) with pagination.

```bash
# First page
betterness lab-records list --json

# Custom page size
betterness lab-records list --limit 50 --page 0 --json

# Second page
betterness lab-records list --limit 20 --page 1 --json
```

| Option | Description |
|--------|-------------|
| `--limit <n>` | Results per page (default: 20) |
| `--page <n>` | Page number, zero-based (default: 0) |

Returns: `externalId`, `source`, and pagination info.

### `lab-records detail`

Get full detail of a lab record by external ID.

```bash
betterness lab-records detail --record-id abc-123 --json
```

| Option | Description |
|--------|-------------|
| `--record-id <id>` | **Required.** Lab record external ID |

## Tips

- Use `list` to find the `externalId`, then `detail` for full information.
- Records include both manually uploaded PDF results and purchased lab test orders.
