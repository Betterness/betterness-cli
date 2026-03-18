---
name: betterness-knowledge
version: 1.0.0
description: "Search the Betterness knowledge library — files and videos via OpenSearch full-text search."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# knowledge

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

## knowledge

```bash
betterness knowledge <command> [flags]
```

### `knowledge search`

Search Betterness-published knowledge (files and videos). Uses OpenSearch full-text search on title and summary fields.

```bash
# Search by text
betterness knowledge search -s "nutrition" --json

# Only videos
betterness knowledge search --type VIDEO --json

# Only files, sorted A-Z
betterness knowledge search --type FILE --sort ASC --json

# Paginate results
betterness knowledge search -s "sleep" --page 0 --page-size 5 --json

# Oldest content first
betterness knowledge search --sort OLDEST --json
```

| Option | Description |
|--------|-------------|
| `-s, --search <text>` | Full-text search query |
| `--type <type>` | `FILE` or `VIDEO` (default: both) |
| `--sort <method>` | `ASC`, `DESC`, `LATEST`, `OLDEST` (default: `LATEST`) |
| `--page <n>` | Page number, 0-indexed (default: 0) |
| `--page-size <n>` | Results per page (default: 10) |

Returns: `externalId`, `title`, `type`, `authors`, `score` (0-100), `date`, `url`, `coverImageUrl`, `reviewCount`

Pagination info is printed to stderr: `Page 1 — 10 of 42 results`

## Tips

- Use `-s` for full-text search — it matches against title (boosted 3x) and summary.
- Omit `--type` to search both files and videos at once.
- Use `--page` and `--page-size` to paginate through large result sets.
- The `score` field is the average review score (0-100) from the community.
