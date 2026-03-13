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

List available lab tests with prices and included markers.

```bash
# All tests
betterness lab-tests list --json

# Search by name
betterness lab-tests list --query "vitamin" --json

# Popular tests only
betterness lab-tests list --popular --json

# Filter by LOINC slug
betterness lab-tests list --loinc-slug "complete-metabolic-panel" --json
```

| Option | Description |
|--------|-------------|
| `--query <text>` | Search by name or description |
| `--popular` | Only show popular tests |
| `--loinc-slug <slug>` | Filter by LOINC slug |

Returns: `name`, `lab`, `price`, `isPopular`, `objectKey`

## Tips

- The `objectKey` is needed for purchasing — pass it to `betterness purchases buy --test-key <objectKey>`.
- Use `--popular` to show commonly ordered panels.
- Combine with `betterness biomarkers search --range OUT_OF_RANGE` to find tests that cover problematic biomarkers.
