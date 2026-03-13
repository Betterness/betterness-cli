---
name: betterness-purchases
version: 1.0.0
description: "Lab test purchases — payment methods, buy with saved card, or Stripe Checkout."
metadata:
  category: "service"
  requires:
    bins: ["betterness"]
---

# purchases

> **PREREQUISITE:** Read `../betterness-shared/SKILL.md` for auth, global flags, and security rules.

```bash
betterness purchases <command> [flags]
```

## Commands

### `purchases payment-methods`

List saved payment methods (credit/debit cards).

```bash
betterness purchases payment-methods --json
```

Returns: `brand`, `last4`, `expiresAt`, `active`, `externalId`

### `purchases buy`

Purchase a lab test using a saved payment method.

```bash
# Preview
betterness purchases buy --test-key TK-abc --payment-method-id pm-123 --dry-run --json

# Execute
betterness purchases buy --test-key TK-abc --payment-method-id pm-123 --json

# With promo code
betterness purchases buy --test-key TK-abc --payment-method-id pm-123 --promo-code SAVE20 --json
```

| Option | Description |
|--------|-------------|
| `--test-key <key>` | **Required.** Lab test object key (from `lab-tests list`) |
| `--payment-method-id <id>` | **Required.** Payment method external ID (from `payment-methods`) |
| `--promo-code <code>` | Promotion code |
| `--dry-run` | Preview what would be purchased without executing |

### `purchases checkout`

Generate a Stripe Checkout payment link (for users without saved cards).

```bash
# Preview
betterness purchases checkout --test-key TK-abc --success-url https://app.betterness.com/success --cancel-url https://app.betterness.com/cancel --dry-run --json

# Execute
betterness purchases checkout --test-key TK-abc --success-url https://app.betterness.com/success --cancel-url https://app.betterness.com/cancel --json
```

| Option | Description |
|--------|-------------|
| `--test-key <key>` | **Required.** Lab test object key |
| `--success-url <url>` | **Required.** Redirect URL after successful payment |
| `--cancel-url <url>` | **Required.** Redirect URL if checkout is cancelled |
| `--promo-code <code>` | Promotion code |
| `--dry-run` | Preview the checkout request without executing |

Returns: `checkoutUrl`, `testName`, `originalPrice`, `finalPrice`, `discount`, `promotionCode`

**Agent rule:** Always use `--dry-run` first for purchases. Confirm amount and test name with the user before executing.

## Tips

- Check `payment-methods` first — if the user has a saved card, use `buy`. Otherwise, use `checkout` for a Stripe payment link.
- The `test-key` comes from `betterness lab-tests list --json` (`objectKey` field).
- After purchase, the order must be initialized with `betterness lab-orders initialize` before scheduling.
