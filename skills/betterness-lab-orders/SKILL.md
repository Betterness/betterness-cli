---
name: betterness-lab-orders
version: 1.0.0
description: "Lab order lifecycle — initialize, find service centers, book/reschedule/cancel appointments."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# lab-orders

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness lab-orders <command> [flags]
```

This is a multi-step workflow. The typical flow is:

```
purchase → initialize → service-centers → slots → book
```

## Commands

### `lab-orders initialize`

Initialize a lab order for processing. The order must be in Paid status (after purchase).

```bash
betterness lab-orders initialize --order-id abc-123 --json
betterness lab-orders initialize --order-id abc-123 --dry-run --json
```

| Option | Description |
|--------|-------------|
| `--order-id <id>` | **Required.** Lab order external ID |
| `--dry-run` | Preview without executing |

### `lab-orders service-centers`

Search lab service centers near a ZIP code.

```bash
betterness lab-orders service-centers --order-id abc-123 --zip-code 90210 --json
```

| Option | Description |
|--------|-------------|
| `--zip-code <zip>` | **Required.** ZIP code to search near |
| `--order-id <id>` | **Required.** Lab order external ID |
| `--limit <n>` | Maximum results (default: 6) |
| `--offset <n>` | Pagination offset (default: 0) |

Returns: `name`, `city`, `state`, `distance`, `siteCode`

### `lab-orders slots`

Get available appointment time slots at a service center.

```bash
betterness lab-orders slots --order-id abc-123 --site-code SC001 --timezone America/New_York --json
```

| Option | Description |
|--------|-------------|
| `--site-code <code>` | **Required.** Service center site code (from `service-centers`) |
| `--order-id <id>` | **Required.** Lab order external ID |
| `--timezone <tz>` | **Required.** IANA timezone |
| `--start-date <YYYY-MM-DD>` | Start date for slot search |
| `--range-days <n>` | Number of days to search (default: 7) |

Returns: `date`, `slots` (with `bookingKey` per slot)

### `lab-orders book`

Book a blood draw appointment at a service center.

```bash
# Preview
betterness lab-orders book --order-id abc-123 --booking-key BK456 --timezone America/New_York --dry-run --json

# Confirm
betterness lab-orders book --order-id abc-123 --booking-key BK456 --timezone America/New_York --json
```

| Option | Description |
|--------|-------------|
| `--order-id <id>` | **Required.** Lab order external ID |
| `--booking-key <key>` | **Required.** Booking key from `slots` command |
| `--timezone <tz>` | **Required.** IANA timezone |
| `--dry-run` | Preview the booking without executing |

### `lab-orders reschedule`

Reschedule an existing blood draw appointment.

```bash
betterness lab-orders reschedule --order-id abc-123 --booking-key BK789 --timezone America/New_York --dry-run --json
```

| Option | Description |
|--------|-------------|
| `--order-id <id>` | **Required.** Lab order external ID |
| `--booking-key <key>` | **Required.** New booking key from `slots` |
| `--timezone <tz>` | **Required.** IANA timezone |
| `--dry-run` | Preview the reschedule without executing |

### `lab-orders cancel`

Cancel a blood draw appointment. Run without `--reason-id` to see available cancellation reasons.

```bash
# List cancellation reasons
betterness lab-orders cancel --order-id abc-123 --json

# Cancel with reason
betterness lab-orders cancel --order-id abc-123 --reason-id 2 --json

# Preview cancellation
betterness lab-orders cancel --order-id abc-123 --reason-id 2 --dry-run --json
```

| Option | Description |
|--------|-------------|
| `--order-id <id>` | **Required.** Lab order external ID |
| `--reason-id <id>` | Cancellation reason ID |
| `--dry-run` | Preview the cancellation without executing |

**Agent rule:** Always use `--dry-run` first for `book`, `reschedule`, and `cancel`. Confirm with the user before executing.

## Tips

- The `bookingKey` from `slots` is required for `book` and `reschedule`.
- The `siteCode` from `service-centers` is required for `slots`.
- Always confirm the appointment time and location with the user before booking.
