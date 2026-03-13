---
name: betterness-lab-results
version: 1.0.0
description: "Lab result management — approve, update biomarkers, update metadata, upload."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# lab-results

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness lab-results <command> [flags]
```

## Commands

### `lab-results update-status`

Update lab result status: approve, rollback, or reprocess.

```bash
betterness lab-results update-status --result-id abc-123 --action APPROVE --json
betterness lab-results update-status --result-id abc-123 --action ROLLBACK --json
betterness lab-results update-status --result-id abc-123 --action REPROCESS --json
```

| Option | Description |
|--------|-------------|
| `--result-id <id>` | **Required.** Lab result external ID |
| `--action <action>` | **Required.** `APPROVE`, `ROLLBACK`, or `REPROCESS` |

### `lab-results update-biomarker`

Update or delete a biomarker value within an uploaded lab result.

```bash
# Update a biomarker value
betterness lab-results update-biomarker --biomarker-id bm-456 --result 5.2 --unit "mg/dL" --json

# Delete a biomarker
betterness lab-results update-biomarker --biomarker-id bm-456 --action DELETE --json
```

| Option | Description |
|--------|-------------|
| `--biomarker-id <id>` | **Required.** Biomarker external ID |
| `--action <action>` | Set to `DELETE` to remove the biomarker |
| `--name <name>` | Biomarker name |
| `--result <value>` | Value (e.g. '5.2', '120') |
| `--unit <unit>` | Unit (e.g. 'mg/dL', 'ng/mL') |
| `--min-range <n>` | Minimum range value |
| `--max-range <n>` | Maximum range value |

### `lab-results update-metadata`

Update metadata of an uploaded lab result (patient info and test details).

```bash
betterness lab-results update-metadata --result-id abc-123 --patient-name "Jane Doe" --lab-name "Quest" --json
```

| Option | Description |
|--------|-------------|
| `--result-id <id>` | **Required.** Lab result external ID |
| `--patient-name <name>` | Patient name |
| `--patient-sex <sex>` | `MALE` or `FEMALE` |
| `--dob <date>` | Date of birth |
| `--lab-name <name>` | Lab name |
| `--ordering-physician <name>` | Ordering physician |
| `--date-collected <date>` | Date collected (ISO-8601) |
| `--fasting` | Mark as fasting test |

### `lab-results upload`

Upload a lab result PDF for processing. The CLI handles the multipart upload directly.

```bash
betterness lab-results upload --file ./my-lab-results.pdf --json
betterness lab-results upload --file /home/user/bloodwork-2025-03.pdf --json
```

| Option | Description |
|--------|-------------|
| `--file <path>` | **Required.** Path to the PDF file |

## Tips

- Use `upload` to submit a lab result PDF, then `update-status --action APPROVE` to finalize it so biomarkers appear in searches.
- Only PDF files are supported.
- The upload has a 60s timeout for large files.
