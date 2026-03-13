---
name: recipe-order-lab-test
version: 1.0.0
description: "End-to-end lab test ordering — search, purchase, initialize, find center, book appointment."
metadata:
  category: "recipe"
  domain: "health"
  requires:
    bins: ["betterness"]
    skills: ["betterness-lab-tests", "betterness-purchases", "betterness-lab-orders"]
---

# Order a Lab Test (End-to-End)

> **PREREQUISITE:** Load the following skills to execute this recipe: `betterness-lab-tests`, `betterness-purchases`, `betterness-lab-orders`

Walk the user through the full lab test ordering flow, from browsing tests to booking a blood draw appointment.

## Steps

1. **Search for a lab test:**
   ```bash
   betterness lab-tests list --query "comprehensive metabolic" --json
   ```
   Present the available tests with prices. Let the user choose.

2. **Check payment methods:**
   ```bash
   betterness purchases payment-methods --json
   ```
   If the user has a saved card, use `buy`. Otherwise, use `checkout`.

3. **Purchase the test (preview first):**
   ```bash
   # With saved card
   betterness purchases buy --test-key <objectKey> --payment-method-id <pmId> --dry-run --json

   # Or generate a Stripe checkout link
   betterness purchases checkout --test-key <objectKey> --success-url https://app.betterness.com/success --cancel-url https://app.betterness.com/cancel --dry-run --json
   ```
   Show the price and confirm with the user before removing `--dry-run`.

4. **Initialize the order:**
   ```bash
   betterness lab-orders initialize --order-id <orderId> --json
   ```

5. **Find nearby service centers:**
   ```bash
   betterness lab-orders service-centers --order-id <orderId> --zip-code <userZip> --json
   ```
   Present options with distance. Let the user choose.

6. **Check available time slots:**
   ```bash
   betterness lab-orders slots --order-id <orderId> --site-code <siteCode> --timezone <userTz> --json
   ```
   Show available dates and times.

7. **Book the appointment (preview first):**
   ```bash
   betterness lab-orders book --order-id <orderId> --booking-key <bookingKey> --timezone <userTz> --dry-run --json
   ```
   Confirm date, time, and location with the user before removing `--dry-run`.

## Important

- Always use `--dry-run` before purchase and booking steps.
- Ask the user for their ZIP code and timezone if not known.
- The `orderId` comes from the purchase response, the `siteCode` from service-centers, and the `bookingKey` from slots.
