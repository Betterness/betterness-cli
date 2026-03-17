---
name: betterness-health-profile
description: "Read, update, and explore the user's health profile questionnaire (sections, questions, answers)."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# health-profile

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness health-profile <command> [flags]
```

## Commands

### `health-profile schema`

List all sections and question IDs. **Always call this first** to discover available fields before reading or writing.

```bash
# List all sections
betterness health-profile schema --json

# List questions in a specific section
betterness health-profile schema --section HWP --json
```

| Option | Description |
|--------|-------------|
| `--section <acronym>` | Show questions for a specific section only |

**Sections:** HWP (Health & Wellness Profile), WEALH (Wellness Goals & Assessments), DMH (Medical History), DA (Diets & Allergies), FHSS (Family Health), FHIS (Family History), WHT (Wearables & Health Tracking), BS (Body Snapshot), PRSUP (Prescriptions & Supplements)

### `health-profile get`

Retrieve the full health profile as a flat map of question ID → answer value.

```bash
betterness health-profile get --json
```

### `health-profile get-section`

Retrieve answers for a single section.

```bash
betterness health-profile get-section --section DA --json
```

| Option | Description |
|--------|-------------|
| `--section <acronym>` | **Required.** Section acronym (e.g. HWP, DMH, DA) |

### `health-profile update`

Patch health profile answers. Only provided question IDs are changed. Accepts a free-form JSON object where keys are question IDs and values are the answer data.

```bash
# Preview changes
betterness health-profile update --dry-run --json \
  --data '{"HWP_HAB_do_you_smoke_cigarettes_or_use_tobacco_products": false}'

# Apply changes
betterness health-profile update --json \
  --data '{"HWP_HAB_do_you_smoke_cigarettes_or_use_tobacco_products": false}'
```

| Option | Description |
|--------|-------------|
| `--data <json>` | **Required.** JSON object with question IDs as keys |
| `--dry-run` | Preview changes without applying |

**Answer value formats** (use `schema` to see the exact type and example for each question):
- **STRING:** `"text value"`
- **BOOLEAN:** `true` or `false`
- **NUMBER_ONE_TO_TEN:** `7`
- **STRING_OPTIONS:** `"Option A"`
- **ARRAY_STRING:** `["Option A", "Option B"]`
- **MAP_ONE_TO_FIVE:** `{"Area 1": "3", "Area 2": "5"}`
- **BOOLEAN_STRING:** `{"boolean": true, "string": "additional details"}`
- **BOOLEAN_ARRAY_STRING:** `{"boolean": true, "arrayString": ["opt1", "opt2"]}`
- **BOOLEAN_KEY_VALUE_LIST:** `{"boolean": true, "keyValueList": [{"name": "Metformin", "value": "500", "dosageUnit": "mg"}]}`
- **BOOLEAN_ARRAY_OBJECT_WITH_DOSIFICATION:** `{"boolean": true, "supplementsData": {"Vitamin D": {"isActive": true, "dosifications": [{"name": "Vitamin D3", "dosage": 1000, "dosageUnit": "IU"}]}}}`
- **BOOLEAN_DOSIFICATION_LIST:** `{"boolean": true, "dosifications": [{"name": "Medication", "dosage": 100, "dosageUnit": "mg"}]}`
- **ARRAY_OBJECT:** `[{"id": "1", "relationship": "Brother", "name": "John", "sex": "Male", "livingStatus": "Living", "healthConditionsStatus": "Known", "healthConditions": ["Diabetes"]}]`

### `health-profile reset-section`

Clear all answers in a section.

```bash
# Preview
betterness health-profile reset-section --section DA --dry-run --json

# Apply
betterness health-profile reset-section --section DA --json
```

| Option | Description |
|--------|-------------|
| `--section <acronym>` | **Required.** Section acronym |
| `--dry-run` | Preview without applying |

### `health-profile summary`

Human-readable summary of all answered questions, organized by section.

```bash
# All sections
betterness health-profile summary --json

# Single section
betterness health-profile summary --section HWP --json
```

| Option | Description |
|--------|-------------|
| `--section <acronym>` | Summarize a specific section only |

Returns rows with `section`, `question` (label), and `answer` (formatted value).

## Agent workflow

1. **Discover:** `health-profile schema --json` → learn available sections and question IDs
2. **Read:** `health-profile get --json` or `get-section --section X --json` → see current values and their data shapes
3. **Write:** `health-profile update --dry-run --data '{...}' --json` → preview, then confirm with user, then apply without `--dry-run`

**Agent rules:**
- Always call `schema` before writing to validate question IDs exist
- Always use `--dry-run` first on `update` and `reset-section`, then confirm with the user
- Use `get` or `get-section` to inspect the current answer format before constructing update data
- Health data is sensitive — always confirm changes with the user before applying
