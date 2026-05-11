---
name: betterness-lab-tests
version: 1.0.0
description: "Browse and search available lab tests with prices and included markers."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# lab-tests

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness lab-tests <command> [flags]
```

## Commands

### `lab-tests list`

List available lab tests purchasable in the user's confirmed US state. `--us-state` is **required** — lab availability and price are state-specific (NY and NJ have different catalogs than other states), so the CLI fails closed without it.

```bash
# Tests purchasable in NJ
betterness lab-tests list --us-state NJ --json

# Search by name within the state catalog
betterness lab-tests list --us-state NJ --query "vitamin" --json

# Popular tests only
betterness lab-tests list --us-state NJ --popular --json

# Filter by LOINC slug
betterness lab-tests list --us-state NJ --loinc-slug "complete-metabolic-panel" --json
```

| Option | Description |
|--------|-------------|
| `--us-state <code>` | **Required.** Two-letter US state code (e.g. `NY`, `NJ`, `FL`). |
| `--query <text>` | Search by name or description |
| `--popular` | Only show popular tests |
| `--loinc-slug <slug>` | Filter by LOINC slug |

Returns: `name`, `lab`, `price`, `isPopular`, `objectKey`

## Tips

- Always confirm the user's US state before running this command. Do not guess: panels and prices differ across states, and same-named panels can be different sellable products in NY vs NJ vs other states.
- The `objectKey` is the canonical identity — pass it to `betterness purchases buy --test-key <objectKey>`. Do not match by `name` alone.
- Use `--popular` to show commonly ordered panels.
- Combine with `betterness biomarkers search --range OUT_OF_RANGE` to find tests that cover problematic biomarkers.
