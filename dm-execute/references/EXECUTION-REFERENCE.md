# Execution Reference

Use this file when you need operational detail beyond the core execution workflow.

## Vendor Types

- Web-to-print: simple formats and lower budgets
- Regional print shop: mid-volume work and closer collaboration
- National print house: higher volume and process consistency
- Variable-data printer: personalized pieces
- Programmatic platform: triggered and low-volume one-to-one sends

## Print-Spec Checklist

- Confirm required PDF format with the printer
- Export in CMYK
- Use print-ready image resolution
- Include bleed and safe zones
- Embed or outline fonts
- Keep the address area and barcode area clean

## Proof Approval

Review in layers:
- digital layout proof
- color or substrate proof when brand color matters
- variable-data proof for edge cases
- written sign-off before production

## Mail House Questions

### Operations

- USPS and address-certification capabilities
- throughput and job minimums
- variable-data handling
- file-security practices

### Pricing

- setup fees
- minimum charges
- postage invoicing method
- whether postal discounts are passed through

### Quality

- mail run report availability
- USPS handoff timing
- defect handling process

## Postage-Class Guidance

### First Class

- Faster and more predictable
- Better when address handling matters
- Higher cost, so reserve for higher-value segments or timing-sensitive mail

### Marketing Mail

- Lower cost
- Best for many acquisition and retention drops
- Less predictable delivery and weaker undeliverable handling

### Local saturation programs

- Strong for geographic reach
- Weaker for suppression precision and personalization

## Partner Governance and Data Handling

Treat external partners as operational dependencies.

Before sharing data or files, confirm:
- scope and owner
- approval path
- data-sharing expectations
- retention and deletion expectations
- escalation contacts
- fallback plan if the partner fails

Scale diligence to the risk level. Higher-risk partners deserve tighter checks around access controls, retention, incident handling, and continuity.

## In-Home Tracking

### Seed list

- Seed a small set of addresses you control
- Spread them across postal areas when useful
- Record seed IDs and arrival dates

### USPS delivery visibility

- Use available postal reporting or informed-delivery-style programs when helpful
- Coordinate reporting start dates with actual drop timing

## Common Execution Failures

- Skipping NCOA or hygiene
- Extracting holdout after the mailed file is finalized
- Activating codes or URLs too late
- Exporting RGB files to print
- Failing to test QR destinations end to end
- Using live credentials during testing
- Forgetting to pass suppression logic to production partners
