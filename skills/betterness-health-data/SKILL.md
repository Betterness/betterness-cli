---
name: betterness-health-data
version: 1.0.0
description: "Activity, sleep, vitals, and body composition from connected wearables."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# Health Data Commands

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

Four commands share the same date-range pattern for querying wearable data:

```bash
betterness <category> get --from <YYYY-MM-DD> --to <YYYY-MM-DD> --timezone <tz> --json
```

| Option | Description |
|--------|-------------|
| `--from <YYYY-MM-DD>` | Start date (default: today) |
| `--to <YYYY-MM-DD>` | End date (default: same as --from) |
| `--timezone <tz>` | IANA timezone (default: system timezone) |

## activity get

Retrieve activity and workout data: steps, distance, calories, VO2 max, workouts.

```bash
betterness activity get --json
betterness activity get --from 2025-03-01 --to 2025-03-07 --json
```

## sleep get

Retrieve nightly sleep data: time in bed, total sleep, sleep stage breakdown.

```bash
betterness sleep get --json
betterness sleep get --from 2025-03-10 --to 2025-03-12 --json
```

## sleep stages

Retrieve minute-by-minute sleep stage transitions (Deep, Core, REM, Awake).

```bash
betterness sleep stages --json
betterness sleep stages --from 2025-03-10 --json
```

## vitals get

Retrieve vital signs: heart rate, HRV, blood pressure, SpO2, glucose, respiratory rate.

```bash
betterness vitals get --json
betterness vitals get --from 2025-03-01 --to 2025-03-07 --json
```

## body-composition get

Retrieve body composition: weight, body fat %, muscle mass, BMI, waist circumference.

```bash
betterness body-composition get --json
betterness body-composition get --from 2025-03-01 --to 2025-03-07 --json
```

## Tips

- All health data requires at least one connected device — check with `betterness connected-devices list --json`.
- Omit `--to` to get a single day's data.
- Use multi-day ranges for trend analysis.
- Data availability depends on the connected wearable (not all devices report all metrics).
