# Betterness CLI Reference

> Auto-generated from command definitions — do not edit manually.
> Run `npm run docs` in `modules/betterness-cli` to regenerate.

Version: 0.0.0

## Global Options

These options apply to all commands:

| Option | Description |
|--------|-------------|
| `-V, --version` | output the version number |
| `--api-key <key>` | API key (overrides env and stored credentials) |
| `--json` | Output as JSON |
| `--markdown` | Output as Markdown |
| `--quiet` | Suppress output (exit code only) |

## Commands

- **[auth](#auth)** — Manage authentication
  - `auth login` — Authenticate with Betterness (OAuth by default, or --key for API key)
  - `auth logout` — Remove stored credentials
  - `auth whoami` — Show the currently authenticated user
- **[profile](#profile)** — User profile information
  - `profile get` — Retrieve current user profile (name, email, phone, gender, DOB, address)
  - `profile update` — Update profile information (only provided fields are changed)
- **[biomarkers](#biomarkers)** — Biomarker lab results and LOINC codes
  - `biomarkers search` — Search and filter biomarker lab results
  - `biomarkers loinc-codes` — List all available LOINC codes for biomarker identification
- **[biological-age](#biological-age)** — Biological age calculations and history
  - `biological-age get` — Get biological age history with biomarker values
- **[activity](#activity)** — Activity and workout data from connected wearables
  - `activity get` — Retrieve activity and workout data (steps, distance, calories, VO2 max, workouts)
- **[sleep](#sleep)** — Sleep data from connected wearables
  - `sleep get` — Retrieve nightly sleep data (time in bed, total sleep, sleep stage breakdown)
  - `sleep stages` — Retrieve minute-by-minute sleep stage transitions (Deep, Core, REM, Awake)
- **[vitals](#vitals)** — Vital signs from connected wearables
  - `vitals get` — Retrieve vital signs (heart rate, HRV, blood pressure, SpO2, glucose, respiratory rate)
- **[body-composition](#body-composition)** — Body composition data from connected wearables
  - `body-composition get` — Retrieve body composition (weight, body fat %, muscle mass, BMI, waist circumference)
- **[connected-devices](#connected-devices)** — Health device integrations and wearable connections
  - `connected-devices list` — List all connected health devices and wearables
  - `connected-devices available` — List health device integrations the user can connect (not currently active)
  - `connected-devices link` — Generate connection link for a web-based health device integration
  - `connected-devices apple-health-code` — Generate connection code for Apple HealthKit via Junction app
  - `connected-devices disconnect` — Disconnect a health device integration
- **[lab-tests](#lab-tests)** — Available lab tests for ordering
  - `lab-tests list` — List available lab tests with prices and included markers
- **[lab-records](#lab-records)** — Lab records — uploaded results and purchased test orders
  - `lab-records list` — List lab records (both uploaded results and lab orders)
  - `lab-records detail` — Get full detail of a lab record by external ID
- **[lab-orders](#lab-orders)** — Lab order management — scheduling, appointments, and service centers
  - `lab-orders initialize` — Initialize a lab order for processing (order must be in Paid status)
  - `lab-orders service-centers` — Search lab service centers near a ZIP code
  - `lab-orders slots` — Get available appointment time slots at a service center
  - `lab-orders book` — Book a blood draw appointment at a service center
  - `lab-orders reschedule` — Reschedule an existing blood draw appointment
  - `lab-orders cancel` — Cancel a blood draw appointment (run without --reason-id to see available reasons)
- **[lab-results](#lab-results)** — Lab result management — approve, update biomarkers, update metadata
  - `lab-results update-status` — Update lab result status (APPROVE, ROLLBACK, or REPROCESS)
  - `lab-results update-biomarker` — Update or delete a biomarker value within an uploaded lab result
  - `lab-results update-metadata` — Update metadata of an uploaded lab result (patient info and test details)
  - `lab-results upload` — Upload a lab result PDF for processing
- **[purchases](#purchases)** — Lab test purchases and payment methods
  - `purchases payment-methods` — List saved payment methods (credit/debit cards)
  - `purchases buy` — Purchase a lab test using a saved payment method
  - `purchases checkout` — Generate a Stripe Checkout payment link for a lab test (for users without saved cards)
- **[smart-listings](#smart-listings)** — Search and browse wellness providers (SmartListings)
  - `smart-listings search` — Search SmartListings by name, description, tags, or location
  - `smart-listings detail` — Get full details of a SmartListing by ID
- **[workflow](#workflow)** — Composite commands that aggregate data from multiple endpoints
  - `workflow daily-brief` — Daily health summary — profile, biological age, biomarkers, devices
  - `workflow next-actions` — Recommended next actions based on biomarkers and health data
- **[schema](#schema)** — Discover available commands, options, and response formats
- **[debug](#debug)** — Internal debug utilities
  - `debug config` — Show resolved configuration
- **[health-profile](#health-profile)** — Health profile questionnaire (read, update, schema)
  - `health-profile schema` — List all sections and question IDs available in the health profile
  - `health-profile get` — Retrieve the full health profile (all answered questions as a flat map)
  - `health-profile get-section` — Retrieve health profile answers for a specific section
  - `health-profile update` — Update health profile questions (only provided fields are changed)
  - `health-profile reset-section` — Clear all answers in a section
  - `health-profile summary` — Human-readable summary of answered health profile questions
- **[mcp](#mcp)** — MCP server integration for AI agents (Claude, Cursor, Windsurf)
  - `mcp install` — Install Betterness MCP integration into an AI client
  - `mcp uninstall` — Remove Betterness MCP integration from an AI client
  - `mcp status` — Show MCP integration status across all supported clients

---

## auth

Manage authentication

### `betterness auth login`

Authenticate with Betterness (OAuth by default, or --key for API key)

| Option | Description |
|--------|-------------|
| `--key <apiKey>` | Use an API key instead of OAuth |

### `betterness auth logout`

Remove stored credentials

### `betterness auth whoami`

Show the currently authenticated user

## profile

User profile information

### `betterness profile get`

Retrieve current user profile (name, email, phone, gender, DOB, address)

### `betterness profile update`

Update profile information (only provided fields are changed)

| Option | Description |
|--------|-------------|
| `--first-name <name>` | First name |
| `--last-name <name>` | Last name |
| `--phone <number>` | Phone number without dial code |
| `--phone-dial-code <code>` | Phone dial code (e.g. +1, +44) |
| `--gender <value>` | Gender: MALE, FEMALE, OTHER, or PREF_NOT |
| `--birth-date <YYYY-MM-DD>` | Date of birth |
| `--address <street>` | Home street address |
| `--city <city>` | City |
| `--state <state>` | State or province |
| `--zip-code <zip>` | ZIP or postal code |
| `--country <code>` | Country (e.g. US, GB) |
| `--dry-run` | Preview changes without applying |

## biomarkers

Biomarker lab results and LOINC codes

### `betterness biomarkers search`

Search and filter biomarker lab results

| Option | Description |
|--------|-------------|
| `--name <text>` | Filter by biomarker name |
| `--loinc-code <code>` | Filter by LOINC code |
| `--start-date <YYYY-MM-DD>` | Start date (ISO-8601) |
| `--end-date <YYYY-MM-DD>` | End date (ISO-8601) |
| `--categories <list>` | Comma-separated category filter |
| `--range <type>` | Range filter: OPTIMAL, AVERAGE, OUT_OF_RANGE, UNKNOWN |
| `--limit <n>` | Maximum number of results (default: `20`) |

### `betterness biomarkers loinc-codes`

List all available LOINC codes for biomarker identification

## biological-age

Biological age calculations and history

### `betterness biological-age get`

Get biological age history with biomarker values

| Option | Description |
|--------|-------------|
| `--limit <n>` | Maximum number of results (default: `10`) |

## activity

Activity and workout data from connected wearables

### `betterness activity get`

Retrieve activity and workout data (steps, distance, calories, VO2 max, workouts)

| Option | Description |
|--------|-------------|
| `--from <YYYY-MM-DD>` | Start date (default: `2026-03-17`) |
| `--to <YYYY-MM-DD>` | End date |
| `--timezone <tz>` | IANA timezone (default: `America/Mendoza`) |

## sleep

Sleep data from connected wearables

### `betterness sleep get`

Retrieve nightly sleep data (time in bed, total sleep, sleep stage breakdown)

| Option | Description |
|--------|-------------|
| `--from <YYYY-MM-DD>` | Start date (default: `2026-03-17`) |
| `--to <YYYY-MM-DD>` | End date |
| `--timezone <tz>` | IANA timezone (default: `America/Mendoza`) |

### `betterness sleep stages`

Retrieve minute-by-minute sleep stage transitions (Deep, Core, REM, Awake)

| Option | Description |
|--------|-------------|
| `--from <YYYY-MM-DD>` | Start date (default: `2026-03-17`) |
| `--to <YYYY-MM-DD>` | End date |
| `--timezone <tz>` | IANA timezone (default: `America/Mendoza`) |

## vitals

Vital signs from connected wearables

### `betterness vitals get`

Retrieve vital signs (heart rate, HRV, blood pressure, SpO2, glucose, respiratory rate)

| Option | Description |
|--------|-------------|
| `--from <YYYY-MM-DD>` | Start date (default: `2026-03-17`) |
| `--to <YYYY-MM-DD>` | End date |
| `--timezone <tz>` | IANA timezone (default: `America/Mendoza`) |

## body-composition

Body composition data from connected wearables

### `betterness body-composition get`

Retrieve body composition (weight, body fat %, muscle mass, BMI, waist circumference)

| Option | Description |
|--------|-------------|
| `--from <YYYY-MM-DD>` | Start date (default: `2026-03-17`) |
| `--to <YYYY-MM-DD>` | End date |
| `--timezone <tz>` | IANA timezone (default: `America/Mendoza`) |

## connected-devices

Health device integrations and wearable connections

### `betterness connected-devices list`

List all connected health devices and wearables

### `betterness connected-devices available`

List health device integrations the user can connect (not currently active)

### `betterness connected-devices link`

Generate connection link for a web-based health device integration

| Option | Description |
|--------|-------------|
| `--integration-key <key>` | Integration provider (GARMIN, OURA, WITHINGS, PELOTON, WAHOO, EIGHT_SLEEP) **(required)** |

### `betterness connected-devices apple-health-code`

Generate connection code for Apple HealthKit via Junction app

### `betterness connected-devices disconnect`

Disconnect a health device integration

| Option | Description |
|--------|-------------|
| `--integration-key <key>` | Integration provider to disconnect **(required)** |
| `--dry-run` | Preview the disconnection without executing |

## lab-tests

Available lab tests for ordering

### `betterness lab-tests list`

List available lab tests with prices and included markers

| Option | Description |
|--------|-------------|
| `--query <text>` | Search by name or description |
| `--popular` | Only show popular tests |
| `--loinc-slug <slug>` | Filter by LOINC slug |

## lab-records

Lab records — uploaded results and purchased test orders

### `betterness lab-records list`

List lab records (both uploaded results and lab orders)

| Option | Description |
|--------|-------------|
| `--limit <n>` | Results per page (default: `20`) |
| `--page <n>` | Page number (zero-based) (default: `0`) |

### `betterness lab-records detail`

Get full detail of a lab record by external ID

| Option | Description |
|--------|-------------|
| `--record-id <id>` | Lab record external ID **(required)** |

## lab-orders

Lab order management — scheduling, appointments, and service centers

### `betterness lab-orders initialize`

Initialize a lab order for processing (order must be in Paid status)

| Option | Description |
|--------|-------------|
| `--order-id <id>` | Lab order external ID **(required)** |
| `--dry-run` | Preview the action without executing |

### `betterness lab-orders service-centers`

Search lab service centers near a ZIP code

| Option | Description |
|--------|-------------|
| `--zip-code <zip>` | ZIP code to search near **(required)** |
| `--order-id <id>` | Lab order external ID **(required)** |
| `--limit <n>` | Maximum results (default: `6`) |
| `--offset <n>` | Pagination offset (default: `0`) |

### `betterness lab-orders slots`

Get available appointment time slots at a service center

| Option | Description |
|--------|-------------|
| `--site-code <code>` | Service center site code **(required)** |
| `--order-id <id>` | Lab order external ID **(required)** |
| `--timezone <tz>` | IANA timezone **(required)** |
| `--start-date <YYYY-MM-DD>` | Start date for slot search |
| `--range-days <n>` | Number of days to search (default: `7`) |

### `betterness lab-orders book`

Book a blood draw appointment at a service center

| Option | Description |
|--------|-------------|
| `--order-id <id>` | Lab order external ID **(required)** |
| `--booking-key <key>` | Booking key from slots command **(required)** |
| `--timezone <tz>` | IANA timezone **(required)** |
| `--dry-run` | Preview the booking without executing |

### `betterness lab-orders reschedule`

Reschedule an existing blood draw appointment

| Option | Description |
|--------|-------------|
| `--order-id <id>` | Lab order external ID **(required)** |
| `--booking-key <key>` | New booking key from slots command **(required)** |
| `--timezone <tz>` | IANA timezone **(required)** |
| `--dry-run` | Preview the reschedule without executing |

### `betterness lab-orders cancel`

Cancel a blood draw appointment (run without --reason-id to see available reasons)

| Option | Description |
|--------|-------------|
| `--order-id <id>` | Lab order external ID **(required)** |
| `--reason-id <id>` | Cancellation reason ID |
| `--dry-run` | Preview the cancellation without executing |

## lab-results

Lab result management — approve, update biomarkers, update metadata

### `betterness lab-results update-status`

Update lab result status (APPROVE, ROLLBACK, or REPROCESS)

| Option | Description |
|--------|-------------|
| `--result-id <id>` | Lab result external ID **(required)** |
| `--action <action>` | Action: APPROVE, ROLLBACK, or REPROCESS **(required)** |

### `betterness lab-results update-biomarker`

Update or delete a biomarker value within an uploaded lab result

| Option | Description |
|--------|-------------|
| `--biomarker-id <id>` | Biomarker external ID **(required)** |
| `--action <action>` | Set to DELETE to remove the biomarker |
| `--name <name>` | Biomarker name |
| `--result <value>` | Biomarker value (e.g. '5.2', '120') |
| `--unit <unit>` | Unit (e.g. 'mg/dL', 'ng/mL') |
| `--min-range <n>` | Minimum range value |
| `--max-range <n>` | Maximum range value |

### `betterness lab-results update-metadata`

Update metadata of an uploaded lab result (patient info and test details)

| Option | Description |
|--------|-------------|
| `--result-id <id>` | Lab result external ID **(required)** |
| `--patient-name <name>` | Patient name |
| `--patient-sex <sex>` | Patient sex: MALE or FEMALE |
| `--dob <date>` | Date of birth |
| `--lab-name <name>` | Lab name |
| `--ordering-physician <name>` | Ordering physician |
| `--date-collected <date>` | Date collected (ISO-8601) |
| `--fasting` | Mark as fasting test |

### `betterness lab-results upload`

Upload a lab result PDF for processing

| Option | Description |
|--------|-------------|
| `--file <path>` | Path to the PDF file **(required)** |

## purchases

Lab test purchases and payment methods

### `betterness purchases payment-methods`

List saved payment methods (credit/debit cards)

### `betterness purchases buy`

Purchase a lab test using a saved payment method

| Option | Description |
|--------|-------------|
| `--test-key <key>` | Lab test object key **(required)** |
| `--payment-method-id <id>` | Payment method external ID **(required)** |
| `--promo-code <code>` | Promotion code |
| `--dry-run` | Preview what would be purchased without executing |

### `betterness purchases checkout`

Generate a Stripe Checkout payment link for a lab test (for users without saved cards)

| Option | Description |
|--------|-------------|
| `--test-key <key>` | Lab test object key **(required)** |
| `--success-url <url>` | URL to redirect after successful payment **(required)** |
| `--cancel-url <url>` | URL to redirect if checkout is cancelled **(required)** |
| `--promo-code <code>` | Promotion code |
| `--dry-run` | Preview the checkout request without executing |

## smart-listings

Search and browse wellness providers (SmartListings)

### `betterness smart-listings search`

Search SmartListings by name, description, tags, or location

| Option | Description |
|--------|-------------|
| `--query <text>` | Text search query |
| `--lat <n>` | Latitude for proximity search |
| `--lng <n>` | Longitude for proximity search |
| `--radius <km>` | Radius in km (1, 2, 5, 10, 20) |
| `--following` | Only show followed providers |
| `--limit <n>` | Results per page (default: `10`) |
| `--page <n>` | Page number (zero-based) (default: `0`) |

### `betterness smart-listings detail`

Get full details of a SmartListing by ID

| Option | Description |
|--------|-------------|
| `--id <externalId>` | SmartListing external ID **(required)** |

## workflow

Composite commands that aggregate data from multiple endpoints

### `betterness workflow daily-brief`

Daily health summary — profile, biological age, biomarkers, devices

| Option | Description |
|--------|-------------|
| `--biomarker-limit <n>` | Maximum biomarkers to fetch (default: `20`) |

### `betterness workflow next-actions`

Recommended next actions based on biomarkers and health data

| Option | Description |
|--------|-------------|
| `--biomarker-limit <n>` | Maximum biomarkers to analyze (default: `50`) |

### `betterness schema`

Discover available commands, options, and response formats

## debug

Internal debug utilities

### `betterness debug config`

Show resolved configuration

## health-profile

Health profile questionnaire (read, update, schema)

### `betterness health-profile schema`

List all sections and question IDs available in the health profile

| Option | Description |
|--------|-------------|
| `--section <acronym>` | Show questions for a specific section only |

### `betterness health-profile get`

Retrieve the full health profile (all answered questions as a flat map)

### `betterness health-profile get-section`

Retrieve health profile answers for a specific section

| Option | Description |
|--------|-------------|
| `--section <acronym>` | Section acronym (e.g. HWP, DMH, DA) **(required)** |

### `betterness health-profile update`

Update health profile questions (only provided fields are changed)

| Option | Description |
|--------|-------------|
| `--data <json>` | JSON object with question IDs as keys and answers as values **(required)** |
| `--dry-run` | Preview changes without applying |

### `betterness health-profile reset-section`

Clear all answers in a section

| Option | Description |
|--------|-------------|
| `--section <acronym>` | Section acronym (e.g. HWP, DMH, DA) **(required)** |
| `--dry-run` | Preview without applying |

### `betterness health-profile summary`

Human-readable summary of answered health profile questions

| Option | Description |
|--------|-------------|
| `--section <acronym>` | Summarize a specific section only |

## mcp

MCP server integration for AI agents (Claude, Cursor, Windsurf)

### `betterness mcp install`

Install Betterness MCP integration into an AI client

```bash
betterness mcp install <client> [--scope <scope>] [--dry-run]
```

| Argument / Option | Description |
|-------------------|-------------|
| `<client>` | Client to configure: `claude`, `claude-code`, `cursor`, `windsurf` **(required)** |
| `--scope <scope>` | Config scope: `global` or `project` (claude-code only, default: `global`) |
| `--dry-run` | Print config without writing |

**Behavior:**
1. Resolves stored credentials (or `--api-key` override)
2. Reads the client's config file (if it exists), merges `mcpServers.betterness` entry
3. Backs up original file as `.bak`
4. Writes updated config

**Client config file locations:**

| Client | macOS | Linux |
|--------|-------|-------|
| `claude` | `~/Library/Application Support/Claude/claude_desktop_config.json` | `~/.config/Claude/claude_desktop_config.json` |
| `claude-code` (global) | `~/.claude/settings.json` | `~/.claude/settings.json` |
| `claude-code` (project) | `.mcp.json` | `.mcp.json` |
| `cursor` | `.cursor/mcp.json` | `.cursor/mcp.json` |
| `windsurf` | `~/.codeium/windsurf/mcp_config.json` | `~/.codeium/windsurf/mcp_config.json` |

### `betterness mcp uninstall`

Remove Betterness MCP integration from an AI client

```bash
betterness mcp uninstall <client> [--scope <scope>]
```

| Argument / Option | Description |
|-------------------|-------------|
| `<client>` | Client to unconfigure: `claude`, `claude-code`, `cursor`, `windsurf` **(required)** |
| `--scope <scope>` | Config scope: `global` or `project` (claude-code only, default: `global`) |

### `betterness mcp status`

Show MCP integration status across all supported clients. Scans all known config file paths and reports which clients have Betterness MCP configured.

```bash
betterness mcp status --json
```

Returns: `client`, `status`, `apiKey` (truncated), `configFile`

