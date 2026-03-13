---
name: betterness-biomarkers
version: 1.0.0
description: "Biomarker lab results, LOINC codes, and biological age calculations."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# biomarkers & biological-age

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

## biomarkers

```bash
betterness biomarkers <command> [flags]
```

### `biomarkers search`

Search and filter biomarker lab results.

```bash
# All recent biomarkers
betterness biomarkers search --json

# Filter by name
betterness biomarkers search --name "Vitamin D" --json

# Out-of-range only
betterness biomarkers search --range OUT_OF_RANGE --json

# By LOINC code and date range
betterness biomarkers search --loinc-code "2085-9" --start-date 2025-01-01 --end-date 2025-12-31 --json

# With category filter
betterness biomarkers search --categories "Lipid Panel,Metabolic" --limit 50 --json
```

| Option | Description |
|--------|-------------|
| `--name <text>` | Filter by biomarker name |
| `--loinc-code <code>` | Filter by LOINC code |
| `--start-date <YYYY-MM-DD>` | Start date (ISO-8601) |
| `--end-date <YYYY-MM-DD>` | End date (ISO-8601) |
| `--categories <list>` | Comma-separated category filter |
| `--range <type>` | `OPTIMAL`, `AVERAGE`, `OUT_OF_RANGE`, or `UNKNOWN` |
| `--limit <n>` | Maximum results (default: 20) |

Returns: `name`, `value`, `unit`, `range`, `date`

### `biomarkers loinc-codes`

List all available LOINC codes for biomarker identification.

```bash
betterness biomarkers loinc-codes --json
```

Returns: `code`, `name`, `categories`

## biological-age

```bash
betterness biological-age <command> [flags]
```

### `biological-age get`

Get biological age history with biomarker values used in the calculation.

```bash
# Latest biological age
betterness biological-age get --limit 1 --json

# History (last 10)
betterness biological-age get --json
```

| Option | Description |
|--------|-------------|
| `--limit <n>` | Maximum results (default: 10) |

Returns: `currentAge`, `biologicalAge`, `source`, `measurementDate`, `calculatedAt`

## Tips

- Use `--range OUT_OF_RANGE` to quickly identify problematic biomarkers.
- Compare `currentAge` vs `biologicalAge` — lower biological age is better.
- Use `biological-age get --limit 1` for the most recent calculation, then `biomarkers search` to see which biomarkers drove the score.
- LOINC codes are standardized identifiers — use them for precise filtering across lab providers.
