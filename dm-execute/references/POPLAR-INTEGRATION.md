# Poplar Integration Reference

This file is intentionally vendor-specific. Use it only when the user is actually working with Poplar.

## Account and Campaign Setup

- Create separate test and production credentials
- Create campaigns by format and postage combination
- Upload HTML creative and confirm merge tags against your data keys
- Render proofs before activation

For format-by-format build guidance, see [Poplar template reference](POPLAR-TEMPLATES.md).

## Core Send Patterns

### Event-triggered send

Use when one customer event should immediately queue one mail piece, such as lapse, sign-up follow-up, or abandoned cart.

Typical inputs:
- campaign identifier
- recipient name and mailing address
- merge tags such as offer text, promo code, or expiration date

### Batch send from a segment

Use when you have a prepared CSV or warehouse extract and want to queue a controlled batch.

Typical controls:
- required-field validation
- skipped-record logging
- chunked submission
- retry logic for transient failures

### Webhook-triggered flow

Use when another system emits an event and your integration decides whether to mail.

Typical gates:
- request authentication
- do-not-mail check
- address-presence check
- result logging

## Example Payload Shape

```json
{
  "campaign_id": "cmp_example",
  "recipient": {
    "first_name": "Jane",
    "last_name": "Smith",
    "address_1": "123 Main St",
    "city": "Austin",
    "state": "TX",
    "postal_code": "78701"
  },
  "merge_tags": {
    "offer_text": "25 percent off your next order",
    "promo_code": "WELCOME25",
    "expiration_date": "April 30, 2026"
  }
}
```

## Operational Guidance

- Respect Poplar's documented rate limits
- Use backoff for retryable errors
- Log mailing identifiers for every accepted send
- Treat validation failures as data issues, not retry candidates
- Monitor batch health and investigate failure spikes quickly
- Keep sandbox and production credentials fully separate
