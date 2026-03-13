---
name: betterness-smart-listings
version: 1.0.0
description: "Search and browse wellness providers (SmartListings)."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# smart-listings

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness smart-listings <command> [flags]
```

## Commands

### `smart-listings search`

Search SmartListings by name, description, tags, or location.

```bash
# Text search
betterness smart-listings search --query "yoga" --json

# Location search
betterness smart-listings search --lat 40.7128 --lng -74.0060 --radius 10 --json

# Only followed providers
betterness smart-listings search --following --json

# Paginated
betterness smart-listings search --query "nutrition" --limit 20 --page 0 --json
```

| Option | Description |
|--------|-------------|
| `--query <text>` | Text search query |
| `--lat <n>` | Latitude for proximity search |
| `--lng <n>` | Longitude for proximity search |
| `--radius <km>` | Radius in km: 1, 2, 5, 10, or 20 |
| `--following` | Only show followed providers |
| `--limit <n>` | Results per page (default: 10) |
| `--page <n>` | Page number, zero-based (default: 0) |

Returns: `name`, `city`, `state`, `rating`, `externalId`

### `smart-listings detail`

Get full details of a SmartListing by ID.

```bash
betterness smart-listings detail --id sl-abc123 --json
```

| Option | Description |
|--------|-------------|
| `--id <externalId>` | **Required.** SmartListing external ID |

## Tips

- Use `search` to find providers, then `detail` with the `externalId` for full information.
- Location search requires both `--lat` and `--lng`. Add `--radius` to control the search area.
