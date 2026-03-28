# Lob Template Reference

This file is intentionally vendor-specific. Use it when the user is designing or debugging Lob templates and creative assets rather than the surrounding API workflow.

Source basis:
- https://docs.lob.com/

## Lob Template Model

Lob supports several creative inputs:
- raw HTML supplied directly in a request
- saved template ids
- template versions
- remote asset URLs
- local file paths where the client library supports them

Lob's reusable-template model is strongest when:
- the same layout is reused across multiple sends
- merge-variable names need to stay stable
- creative updates should be versioned separately from the sending code

Starter assets included in this skill:
- `assets/lob/postcard-6x9-front.html`
- `assets/lob/postcard-6x9-back.html`
- `assets/lob/letter-us-letter.html`
- `assets/lob/self-mailer-12x9-bifold-inside.html`
- `assets/lob/self-mailer-12x9-bifold-outside.html`

## Supported Surfaces By Product

Lob's docs describe these product surfaces:
- postcards: `front` and `back`
- letters: `file`
- self mailers: `inside` and `outside`

That is the key mental model for building with Lob:
- postcards are two-sided
- letters are a document file with address-placement rules
- self mailers are inside and outside spreads, not just flat pages

## Shared HTML Rules From Lob

Lob's template-design guidance emphasizes:
- use inline styles or an internal stylesheet
- do not depend on external stylesheets
- use performant asset hosting with no rate limits
- keep linked assets at final physical size and 300 DPI or better
- keep the total size of linked assets under Lob's documented limit
- review the final rendered PDF in test mode because browser rendering is not the source of truth

Lob also notes:
- merge variables should not include delimiting whitespace
- HTML syntax errors in variables can fail template creation or versioning
- reusable HTML templates are capped by Lob's documented HTML length limits

## Template Workflow

Recommended Lob workflow:
1. Pick the mail product and size first.
2. Decide whether to send raw HTML or use saved templates and versions.
3. Build the creative surface for that product.
4. Render proofs in test mode.
5. Lock template ids or version ids in the sending workflow only after proof approval.

## Postcards

### Product model

- Two surfaces: `front` and `back`
- Supports HTML, template ids, remote files, and local files depending on the client path used

### Size guidance from Lob docs

For PDF, PNG, and JPEG postcard art, Lob documents these art sizes at 300 DPI:
- 4.25 by 6.25 inches
- 6.25 by 9.25 inches
- 6.25 by 11.25 inches

Those dimensions include the bleed border, not just the trimmed face.

### Build notes

- Back art must respect the address and postage area.
- Use the official postcard templates for each size rather than guessing the clear zone.
- HTML postcard art is rendered to the specified Lob size, so the chosen `size` still matters even when the source is HTML.

### Best use cases

- broad acquisition
- simple win-back
- offer-forward programs
- QR-led response

## Letters

### Product model

- One primary surface: `file`
- Address behavior is controlled separately through `address_placement`

### Important letter settings from Lob docs

Lob documents several `address_placement` options, including:
- `top_first_page`
- `insert_blank_page`
- `bottom_first_page_center`
- `bottom_first_page`

Lob also documents:
- `double_sided`
- `color`
- `extra_service` for certified or related workflows
- size defaults that depend on API version behavior

### Build notes

- Address placement is part of the template design, not an afterthought.
- If the address sits on the first page, the layout must reserve that zone.
- If a blank address page is inserted, account for the extra sheet in page flow and cost.
- Certified-mail variants can add extra pages or envelope-specific constraints, so do not reuse a standard letter proof blindly.

### Best use cases

- trust-heavy offers
- high-consideration categories
- long-form copy
- enclosed inserts or cards

## Self Mailers

### Product model

- Two surfaces: `inside` and `outside`
- Supports dedicated self-mailer sizes rather than one generic spread

### Size guidance from Lob docs

Lob documents self-mailer size options including:
- `6x18_bifold`
- `11x9_bifold`
- `12x9_bifold`
- `17.75x9_trifold`

Lob's self-mailer endpoint also supports options such as:
- `qr_code`
- `send_date`
- `print_speed`
- `mail_type`

### Build notes

- Treat self mailers as panel-based pieces, not just large postcards.
- Use the official self-mailer template for the chosen size.
- Validate the outside first, because that is what the recipient sees in the mail stream.
- Keep response logic coherent after the first unfold, not only when fully open.
- If using Lob's QR placement support, confirm the QR position works with the chosen size and page set.

### Best use cases

- richer product storytelling
- service explainers
- panel-sequenced narratives
- programs that need more space without moving to a letter package

## Saved Templates And Versions

Lob documents reusable HTML templates and template versions. Use them when:
- creative updates should be versioned independently
- the same layout is reused frequently
- you want the send request to stay short and stable

Practical pattern:
- create a template per surface or layout
- create new versions for creative changes
- proof the new version in test mode
- promote the new version only after review

## Merge Variables

Use merge variables only after the layout works with fixed content.

Recommended rules:
- keep variable names stable
- avoid whitespace inside delimiters
- define fallbacks for optional fields in the sending system
- proof the longest expected values
- proof empty-value cases

## Proofing And QA

For every Lob template:
- render in test mode
- review the final PDF, not only browser output
- confirm image quality and crop
- confirm address placement for the selected product
- check panel behavior on self mailers
- validate page count, duplex settings, and extra-service implications on letters

## Using The Starter Assets

These starter files are meant to be copied into a Lob template or sent as raw HTML during prototyping.

Before using them live:
- confirm the Lob product and size match the asset filename
- confirm your address-placement choice for letters
- confirm merge-variable keys match your payload exactly
- proof with realistic long values and blank values
- validate all postal clear zones against Lob's official templates for the chosen product

## Format-Specific Failure Modes

### Postcards

- back art overlaps postal zones
- wrong size is paired with the art
- HTML render looks fine in browser but not in Lob output

### Letters

- address placement collides with headline or salutation
- inserted blank page changes pagination unexpectedly
- certified or registered options invalidate a previously approved proof

### Self mailers

- outside panel is weak or cluttered
- panel order is misunderstood
- QR placement conflicts with folds or important copy

## When To Use Raw HTML Versus Saved Templates

Use raw HTML when:
- the creative is short-lived
- the integration is still being prototyped
- the team is moving quickly and version history is not yet important

Use saved templates and template versions when:
- the layout will be reused
- multiple people review creative
- production changes need auditable versioning
- the sending layer should remain stable while creative iterates
