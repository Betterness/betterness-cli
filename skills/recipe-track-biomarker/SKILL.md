---
name: recipe-track-biomarker
version: 1.0.0
description: "Track a specific biomarker over time — history, trends, and when to retest."
metadata:
  category: "recipe"
  domain: "health"
  requires:
    bins: ["betterness"]
    skills: ["betterness-biomarkers", "betterness-lab-tests"]
---

# Track a Biomarker Over Time

> **PREREQUISITE:** Load the following skills to execute this recipe: `betterness-biomarkers`, `betterness-lab-tests`

Help the user track a specific biomarker's history and determine if retesting is needed.

## Steps

1. **Find the biomarker's LOINC code:**
   ```bash
   betterness biomarkers loinc-codes --json
   ```
   Search the output for the biomarker name to get its LOINC code.

2. **Pull historical values:**
   ```bash
   betterness biomarkers search --loinc-code <code> --limit 50 --json
   ```
   Or by name:
   ```bash
   betterness biomarkers search --name "Vitamin D" --limit 50 --json
   ```

3. **Analyze the trend:**
   - Is the value improving, worsening, or stable?
   - Is the latest value in range (OPTIMAL, AVERAGE, OUT_OF_RANGE)?
   - How old is the most recent result?

4. **Suggest retesting if needed:**
   If the latest result is > 90 days old or out of range, search for a relevant lab test:
   ```bash
   betterness lab-tests list --query "<biomarker-name>" --json
   ```
   Offer to help the user order the test (see `recipe-order-lab-test`).

## Presentation

- Show values as a timeline (oldest → newest).
- Highlight the range status of each measurement.
- If improving, encourage the user. If worsening, suggest consulting their provider.
