---
name: betterness-workflow
version: 1.0.0
description: "Composite commands — daily health brief and recommended next actions."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# workflow

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness workflow <command> [flags]
```

Workflow commands aggregate data from multiple endpoints in a single call.

## Commands

### `workflow daily-brief`

Daily health summary combining profile, biological age, biomarkers, and connected devices.

```bash
betterness workflow daily-brief --json
betterness workflow daily-brief --biomarker-limit 50 --json
```

| Option | Description |
|--------|-------------|
| `--biomarker-limit <n>` | Maximum biomarkers to fetch (default: 20) |

Fetches in parallel:
1. User profile
2. Latest biological age
3. Recent biomarkers (highlights out-of-range)
4. Connected devices

### `workflow next-actions`

Recommended next actions based on biomarkers and health data.

```bash
betterness workflow next-actions --json
betterness workflow next-actions --biomarker-limit 100 --json
```

| Option | Description |
|--------|-------------|
| `--biomarker-limit <n>` | Maximum biomarkers to analyze (default: 50) |

Returns prioritized actions:
- **HIGH:** Biomarkers out of range that need attention
- **MEDIUM:** Tests older than 90 days — due for retesting
- **LOW:** No connected devices — limited health data

## Tips

- `daily-brief` is the best starting point for any health conversation — gives full context in one call.
- `next-actions` is ideal for proactive health coaching — suggests what to do next.
- Both commands use `--json` for structured output that's easy to parse and present to the user.
