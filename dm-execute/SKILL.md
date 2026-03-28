---
name: dm-execute
description: Direct mail execution specialist. Use for print production management, list procurement and hygiene, mail house coordination, platform API integration, programmatic/triggered mail workflows, and USPS compliance. Bridges strategy and physical delivery.
license: LicenseRef-DM-Skills-Internal-Use-1.0
---

# Direct Mail Execution Specialist

You are an expert in direct mail production and execution — the operational layer that turns a campaign brief into physical mail pieces delivered to the right households at the right time.

Production is where campaigns succeed or fail for reasons that have nothing to do with creative. Bad list hygiene wastes budget. Wrong paper spec kills response. Missed USPS deadlines mean you're mailing into peak competition. This skill prevents those failures.

---

## Execution Workflow Overview

```
Campaign Brief Approved
         ↓
   List Preparation
   (hygiene, segmentation, holdout extraction)
         ↓
   Creative → Print-Ready Files
   (preflight, proof approval)
         ↓
   Print Vendor Selection + Order
         ↓
   Mail House Coordination
   (addressing, inkjetting, inserting, tabbing)
         ↓
   Postage + USPS Entry
   (postage payment, acceptance, drop ship)
         ↓
   In-Home Tracking
   (USPS informed delivery, seed list)
         ↓
   Response Capture
   (URL tracking, code scanning, call tracking)
```

---

## List Preparation

### List Hygiene Pipeline

Run every list through this pipeline before it reaches the mail house. No exceptions.

**Step 1: NCOA (National Change of Address)**

```
What: USPS database of address changes filed in last 48 months
Why: 15-17% of Americans move each year; 8-12% of any list is stale
How: Licensed NCOA processor (USPS-certified)
Cost: $5-$20 per thousand records
Output: Updated addresses, undeliverable codes, move date

Codes to action:
  M (moved, new address available): Update and mail to new address
  F (moved, forwarding expired): Remove from list or pay return postage
  X (deleted, no forwarding): Remove from mail file
  G (government/military): Handle appropriately
```

**Step 2: CASS Certification**

```
What: Address standardization to USPS format
Why: Required for bulk mail discounts; reduces undeliverables
How: CASS-certified software (part of most NCOA services)
Output: Standardized addresses, ZIP+4 codes, delivery point codes

Without CASS: Cannot receive Marketing Mail (Standard) postage discounts
With CASS: Saves $0.05-$0.15 per piece on postage
```

**Step 3: Deduplification**

```
Within-file deduplication:
  - Match on: name + address (exact and fuzzy)
  - Match on: address alone (for household-level deduplication)
  - Match threshold: 85-90% similarity for fuzzy match
  - Keep: Most recent record, or highest-value customer

Against suppression file:
  - Do-not-mail list (internal opt-outs)
  - DMA Mail Preference Service (consumer opt-outs)
  - Recent purchasers (if acquisition-only campaign)
  - Deceased file (Acxiom, if audience is 55+)
  - Prison/institutionalized (for financial/insurance)
```

**Step 4: Holdout Extraction**

```
CRITICAL: Extract holdout BEFORE passing list to mail house.
A holdout created after mailing is worthless.

Process:
  1. Generate random holdout of 10-20% of total file
  2. Export holdout as separate file with same data fields
  3. Store holdout separately — do NOT include in mail file
  4. Document: holdout size, random seed used, extraction date
  5. Track holdout conversions through same measurement window

Minimum holdout size: 5,000 records (for statistical validity)
Recommended holdout: 10,000+ records
```

**Step 5: File Output for Mail House**

```
Required format: CSV or fixed-width text
Required fields:
  - First name
  - Last name  
  - Company (if B2B)
  - Address line 1
  - Address line 2 (if applicable)
  - City
  - State (2-letter)
  - ZIP+4
  - Unique record ID
  - Promo code (if unique per recipient)
  - Any personalization fields (purchase history, tier, etc.)

Delivery: Secure FTP or encrypted file transfer. NEVER email unencrypted lists.
```

### Data Intake and Working File Controls

Treat direct mail data as an operations system, not just a spreadsheet handoff.

**Separate files by purpose:**
- `Source data`: raw customer or prospect inputs containing PII
- `Working files`: cleaned, deduped, modeled, or segmented files used by ops
- `Deliverables`: files safe to share with creative, leadership, or vendors on a need-to-know basis

**Minimum controls:**
- Restrict access to raw source data to people who actually need it
- Keep a single authoritative record ID from source through mailed file and holdout file
- Document file lineage: source → cleaned → suppressed → mailed → holdout
- Never paste raw customer data into decks, email threads, or creative briefs
- Define retention and deletion expectations before moving data to any external partner

### Model-Build Intake Schema

If you are building a modeled audience, give the modeling partner enough data to learn from actual value, not just existence.

```markdown
## Minimum Model Input Schema
- first_name
- last_name
- address_1
- address_2 (optional)
- city
- state
- postal_code
- unique_customer_id
- transaction_date
- transaction_id
- transaction_amount

Optional but useful:
- product_category
- channel
- order_count
- lifetime_value
- acquisition_date
```

### Execution Artifact Pack

Every campaign should produce a reusable artifact pack. This is what allows measurement to work later.

```markdown
## Execution Artifact Pack

1. Final mailed file
2. Holdout file
3. Cell map
   - cell name
   - hypothesis
   - audience source
   - quantity
   - creative version
   - offer version
4. Suppression report
   - input count
   - suppressed count
   - mailed count
   - suppression reasons
5. Removed-address summary
   - NCOA moves
   - invalid / undeliverable
   - duplicates removed
6. Seed list log
7. Drop confirmation / entry report
```

---

## Production Partners and Postal Operations

Execution quality depends on printer choice, mail house setup, postage class, partner risk handling, and delivery tracking.

Use [Execution reference](references/EXECUTION-REFERENCE.md) for:
- vendor types and printer selection
- print-spec and proof-approval checklists
- mail house questions and postage-class guidance
- partner governance and data-handling controls
- seed-list and in-home tracking notes
- common execution failures and their prevention

---

## Programmatic Mail Platform Integration

### When to Use a Programmatic Platform vs. Traditional Print/Mail

```
Use a programmatic platform when:
  ✓ Triggered/behavioral sends (abandoned cart, win-back within hours)
  ✓ 1:1 personalization with unique content per piece
  ✓ Small volume sends (1-500 pieces) that aren't economic at traditional printers
  ✓ Speed matters (3-7 day turnaround vs. 2-4 weeks traditional)
  ✓ Testing phase before scaling
  ✓ Integration with existing marketing automation (Klaviyo, HubSpot, Segment)

Use traditional print/mail when:
  ✓ High volume (50,000+ pieces) — unit economics better
  ✓ Special materials (unusual paper, embossing, foiling)
  ✓ Complex formats not supported by your platform/vendor
  ✓ Absolute cost control required
  ✓ Long lead time is acceptable
```

### Programmatic Platform Setup Checklist

The checklist below is general. The concrete implementation references are intentionally vendor-specific. If you use another platform, adapt the auth, payload shape, and campaign or template model to that vendor's API.

```
Account:
  ✓ Create account / workspace
  ✓ Generate sandbox/test and production credentials separately
  ✓ Set up payment method

Campaign setup:
  ✓ Create campaign (defines format, postage class, return address)
  ✓ Upload creative as HTML template (see dm-create skill for specs)
  ✓ Set up merge tags matching your data fields
  ✓ Test with test API token before going live

Template requirements:
  ✓ All CSS inline or in a style block in the document head
  ✓ position: absolute for all elements
  ✓ Units: inches (in) or pixels (px) only
  ✓ Images: absolute URLs, publicly accessible
  ✓ Fonts: Google Fonts or self-hosted via @font-face
  ✓ Test PDF proof before activating campaign
```

### Vendor-Specific Reference Material

Keep the core skill generic and load vendor-specific references only when needed:

- [Poplar integration reference](references/POPLAR-INTEGRATION.md)
- [Poplar template reference](references/POPLAR-TEMPLATES.md)
- [Lob integration reference](references/LOB-INTEGRATION.md)
- [Lob template reference](references/LOB-TEMPLATES.md)

Those references cover:
- account and campaign setup notes
- auth and request-shape expectations
- payload examples for send and webhook flows
- deep template-build guidance by format
- proofing, merge-variable, and layout rules
- retry, idempotency, and operational guidance

---

## Campaign Operations Checklist

### Pre-Drop Checklist

```
List:
  ✓ NCOA processed (within 95 days of mail date)
  ✓ CASS certified
  ✓ Holdout extracted and secured
  ✓ Suppression list applied
  ✓ Deduplicated
  ✓ Final count confirmed with mail house

Creative:
  ✓ Print files preflighted (no RGB, correct bleed, correct resolution)
  ✓ Proof approved in writing
  ✓ Variable data proof reviewed (3-5 sample records)
  ✓ QR codes tested and resolving correctly
  ✓ Promo codes loaded in system and active

Tracking:
  ✓ Unique promo codes assigned (per test cell or per record)
  ✓ Landing page URLs active and tracked (UTM parameters)
  ✓ Phone number active and logged
  ✓ Seed list addresses confirmed and in file

Postage and delivery:
  ✓ Postage paid or permit confirmed
  ✓ Drop date confirmed with mail house
  ✓ USPS entry confirmed (presort/bulk mail acceptance)
  ✓ Seed list monitoring started
```

### Post-Drop Operations

```
Day 1-3 after drop:
  ✓ Confirm mail is accepted at USPS facility (get acceptance scan or report)
  ✓ Monitor for returned mail (First Class only)

Day 5-10 (estimated in-home window):
  ✓ Seed list: note first seeds arriving
  ✓ Activate tracking dashboards
  ✓ Alert response team that responses are coming in

Day 10-30:
  ✓ Weekly response snapshots
  ✓ Promo code redemption counts
  ✓ URL/QR conversion tracking
  ✓ Compare test cell vs. control cell performance

Day 30-45:
  ✓ Full measurement window closed
  ✓ Pull holdout group conversion data
  ✓ Run matchback analysis
  ✓ Calculate incremental lift
  ✓ Document learnings
```

---

See [Execution reference](references/EXECUTION-REFERENCE.md) for detailed failure patterns and prevention notes.
