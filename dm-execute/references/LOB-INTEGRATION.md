# Lob Integration Reference

This file is intentionally vendor-specific. Use it only when the user is actually working with Lob.

Source basis:
- https://docs.lob.com/

## Authentication and Versioning

- Lob uses HTTP Basic authentication.
- Use the API key as the username and leave the password blank.
- Keep secret keys server-side only.
- Publishable keys are limited and are not appropriate for print-and-mail sends.
- When relevant, pin a Lob API version explicitly with the `Lob-Version` header.

## Core Product Surfaces

For direct-mail execution, the main Lob API surfaces are:
- postcards
- letters
- self_mailers
- templates and template versions
- webhooks and tracking events
- address verification for pre-send hygiene

## Account and Campaign Setup

- Separate test and live credentials.
- Decide which mail products you are actually sending before wiring templates.
- Set up reusable templates where the same layout will be used repeatedly.
- Confirm merge-variable names match your internal data keys.
- Verify address quality before creating live mail pieces.

For format-by-format build guidance, see [Lob template reference](LOB-TEMPLATES.md).

## Core Send Patterns

### Event-triggered send

Use when a single behavioral event should create one mail piece, such as:
- lapse threshold reached
- onboarding follow-up
- cart abandonment
- high-value lead response

Typical controls:
- verify the recipient is eligible for mail
- verify the address before creating the piece
- pass merge variables for offer, code, timing, and personalization
- store the Lob resource id back in your own system

### Batch send from a segment

Use when you have a prepared audience extract and want to create postcards, letters, or self-mailers in controlled volume.

Typical controls:
- validate required address fields
- run address verification before creation
- apply idempotency keys for safe retries
- log created resource ids and failures by record id

### Webhook-driven monitoring

Use webhooks to track asynchronous mail lifecycle changes and downstream reporting signals.

Typical uses:
- render-complete confirmation
- production and delivery events
- QR or URL view events when available
- failure handling and support workflows

## Example Payload Shape

This is a simplified example shaped like a Lob postcard create request:

```json
{
  "description": "Win-back postcard",
  "to": {
    "name": "Jane Smith",
    "address_line1": "123 Main St",
    "address_city": "Austin",
    "address_state": "TX",
    "address_zip": "78701"
  },
  "from": "adr_example_sender",
  "front": "tmpl_front_example",
  "back": "tmpl_back_example",
  "mail_type": "usps_first_class",
  "merge_variables": {
    "offer_text": "25 percent off your next order",
    "promo_code": "WELCOME25",
    "expiration_date": "April 30, 2026"
  }
}
```

Adapt the product-specific fields for letters or self-mailers as needed.

## Concrete Request Examples

Lob's curl examples commonly use Basic auth plus form-style fields. The examples below follow that pattern so they match the official docs closely.

### Curl: create a postcard from saved addresses and templates

Use saved resource ids when you want the request body to stay simple and repeatable.

```bash
curl https://api.lob.com/v1/postcards \
  -u "$LOB_API_KEY:" \
  -H "Idempotency-Key: winback-cust-123-2026-03-28" \
  -d "description=Win-back postcard" \
  -d "to=adr_recipient_example" \
  -d "from=adr_sender_example" \
  -d "front=tmpl_front_example" \
  -d "back=tmpl_back_example" \
  -d "mail_type=usps_first_class" \
  -d 'merge_variables={"offer_text":"25 percent off your next order","promo_code":"WELCOME25","expiration_date":"April 30, 2026"}'
```

### Python: verify a US address before sending

Use US verification as a delivery-quality gate before you create live mail.

```python
import os
import requests

LOB_API = "https://api.lob.com/v1"
LOB_KEY = os.environ["LOB_API_KEY"]

def verify_us_address():
    response = requests.post(
        f"{LOB_API}/us_verifications",
        auth=(LOB_KEY, ""),
        json={
            "primary_line": "210 King Street",
            "city": "San Francisco",
            "state": "CA",
            "zip_code": "94107"
        },
        params={"case": "proper"},
        timeout=30,
    )
    response.raise_for_status()
    return response.json()
```

Recommended gating:
- stop the send if the address is undeliverable
- review missing-unit or incorrect-unit results before mailing
- persist the normalized address or verification id in your mailed-file lineage

### Python: create a postcard with idempotency and metadata

```python
import json
import os
import requests

LOB_API = "https://api.lob.com/v1"
LOB_KEY = os.environ["LOB_API_KEY"]

def create_postcard(customer_id: str):
    response = requests.post(
        f"{LOB_API}/postcards",
        auth=(LOB_KEY, ""),
        headers={
            "Idempotency-Key": f"winback-{customer_id}-2026-03-28",
        },
        data={
            "description": "Win-back postcard",
            "to": "adr_recipient_example",
            "from": "adr_sender_example",
            "front": "tmpl_front_example",
            "back": "tmpl_back_example",
            "mail_type": "usps_first_class",
            "metadata": json.dumps({
                "customer_id": customer_id,
                "campaign": "winback_q2"
            }),
            "merge_variables": json.dumps({
                "offer_text": "25 percent off your next order",
                "promo_code": "WELCOME25",
                "expiration_date": "April 30, 2026"
            }),
        },
        timeout=30,
    )
    response.raise_for_status()
    return response.json()
```

### TypeScript: create a postcard with `fetch`

```ts
const apiKey = process.env.LOB_API_KEY!;
const auth = Buffer.from(`${apiKey}:`).toString("base64");

const body = new URLSearchParams({
  description: "Win-back postcard",
  to: "adr_recipient_example",
  from: "adr_sender_example",
  front: "tmpl_front_example",
  back: "tmpl_back_example",
  mail_type: "usps_first_class",
  merge_variables: JSON.stringify({
    offer_text: "25 percent off your next order",
    promo_code: "WELCOME25",
    expiration_date: "April 30, 2026",
  }),
});

const response = await fetch("https://api.lob.com/v1/postcards", {
  method: "POST",
  headers: {
    Authorization: `Basic ${auth}`,
    "Idempotency-Key": "winback-cust-123-2026-03-28",
    "Content-Type": "application/x-www-form-urlencoded",
  },
  body,
});

if (!response.ok) {
  throw new Error(`Lob postcard create failed: ${response.status}`);
}

const postcard = await response.json();
```

## Request Design Guidance

- Use idempotency keys on create requests so retries do not duplicate mail.
- Store metadata that maps the Lob resource back to your internal customer or campaign ids.
- Choose mail type intentionally:
  - first class when timing or deliverability matters more
  - standard when cost efficiency matters more and the product supports it
- Remember that some product and size combinations have postage limitations.

## Template and Merge Guidance

- Lob supports reusable HTML templates and template versions.
- Keep merge-variable names stable and easy to map from your source data.
- Validate rendered output before live sends.
- Keep HTML and personalization rules in versioned templates when the layout is reused often.

## Proofing and Scheduling Guidance

- Generate or retrieve proofs before activating new creative at scale.
- Use scheduled sends only when you have a real operational need and a clear cancellation policy.
- If you schedule sends, store the send date and the Lob resource id in the same execution artifact pack you use for mailed and holdout files.
- Treat cancellation windows as part of campaign ops, not as a substitute for pre-drop QA.

## Address Quality Guidance

Use Lob address verification as an execution-time quality gate, not as a substitute for your broader hygiene process.

Recommended pattern:
- internal suppression and dedupe
- address verification
- final mailed-file export or API create step

This keeps the execution workflow aligned with the rest of the skill:
- suppress first
- protect holdout
- verify address quality
- then create the mail piece

## Webhooks and Tracking

Lob documents webhook support for event and tracking notifications.

Useful event families include:
- resource created or rejected
- rendered PDF and thumbnail readiness
- mailed, in transit, local-area, processed-for-delivery, delivered
- rerouted or returned
- viewed or informed-delivery engagement events where available

Recommended webhook controls:
- verify the request source according to Lob's webhook guidance
- log all incoming event ids
- make webhook handling idempotent
- update internal mail status from webhook events rather than polling whenever possible

### Practical status mapping

Normalize Lob events into a simpler internal status model so reporting stays vendor-agnostic.

| Lob signal | Suggested internal status | Use |
|------------|---------------------------|-----|
| `postcard.created` | `queued` | API accepted the mailpiece |
| `postcard.rendered_pdf` | `proof_ready` | Creative can be reviewed or audited |
| `postcard.mailed` | `mailed` | Production complete |
| `postcard.in_transit` | `in_transit` | Delivery underway |
| `postcard.processed_for_delivery` | `delivery_imminent` | Good trigger for monitoring response onset |
| `postcard.delivered` | `delivered` | Final postal delivery event |
| `postcard.failed` or `postcard.rejected` | `failed` | Investigate and retry only if appropriate |
| `postcard.viewed` | `engaged` | Useful for QR or URL interaction tracking |

## Operational Guidance

- Keep test and live traffic fully separated.
- Use address verification and proofing before expensive live sends.
- Persist Lob ids so you can reconcile sends, cancellations, and delivery events later.
- Treat validation failures as data issues, not retry candidates.
- Retry only transient network or service failures, and pair retries with idempotency keys.
- Read rate-limit headers and throttle batch traffic intentionally rather than flooding create endpoints.
