---
name: recipe-health-checkup
version: 1.0.0
description: "Comprehensive daily health review — profile, bio age, biomarkers, wearable data, next actions."
metadata:
  category: "recipe"
  domain: "health"
  requires:
    bins: ["betterness"]
    skills: ["betterness-workflow", "betterness-health-data", "betterness-biomarkers"]
---

# Daily Health Checkup

> **PREREQUISITE:** Load the following skills to execute this recipe: `betterness-workflow`, `betterness-health-data`, `betterness-biomarkers`

Run a comprehensive health review for the user, combining all available data sources.

## Steps

1. **Get the daily brief:**
   ```bash
   betterness workflow daily-brief --biomarker-limit 50 --json
   ```
   Summarize: user info, biological age comparison, out-of-range biomarkers, and connected devices.

2. **Get recent wearable data (last 7 days):**
   ```bash
   betterness activity get --from <7-days-ago> --to <today> --json
   betterness sleep get --from <7-days-ago> --to <today> --json
   betterness vitals get --from <7-days-ago> --to <today> --json
   ```
   Summarize trends: steps, sleep quality, heart rate, HRV.

3. **Get recommended next actions:**
   ```bash
   betterness workflow next-actions --json
   ```
   Present prioritized actions (HIGH/MEDIUM/LOW) with specific recommendations.

4. **Deep dive on out-of-range biomarkers (if any):**
   ```bash
   betterness biomarkers search --range OUT_OF_RANGE --json
   ```
   For each out-of-range biomarker, explain what it means and what the user can do.

## Presentation

- Start with the big picture (biological age vs chronological age).
- Highlight positive trends and improvements.
- Focus actionable items on the highest-priority issues.
- If no connected devices, suggest connecting one for richer data.
