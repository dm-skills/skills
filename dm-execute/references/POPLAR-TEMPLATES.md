# Poplar Template Reference

This file is intentionally vendor-specific. Use it when the user is designing or debugging Poplar creative templates rather than the surrounding API workflow.

Source basis:
- https://docs.heypoplar.com/getting-started/creative-guides/static-upload-guide
- https://docs.heypoplar.com/getting-started/creative-guides/dynamic-html-css
- https://docs.heypoplar.com/getting-started/creative-guides

## Poplar Template Model

Poplar supports two main creative paths:
- static uploads, where you supply finished art files
- dynamic HTML and CSS, where Poplar renders personalized mail from HTML, CSS, and merge data

Use static uploads when:
- the piece is mostly fixed
- design precision matters more than deep personalization
- the creative team is working in print tools first

Use dynamic HTML and CSS when:
- personalization is central
- you need reusable layout logic
- you want merge-variable, conditional, or repeated-content behavior

Starter assets included in this skill:
- `assets/poplar/postcard-6x9-front.html`
- `assets/poplar/postcard-6x9-back.html`
- `assets/poplar/letter-8.5x11.html`
- `assets/poplar/trifold-16.625x8.75.html`

## Supported Formats At A Glance

The Poplar creative guides document these formats:
- 4x6 postcard
- 6x9 postcard
- 6x11 postcard
- 8.5x11 bifold long
- 17x5.5 bifold short
- 16.625x8.75 trifold
- 8.5x11 letter

Operational rule:
- postcards use front and back creative
- folded mailers should be designed as complete spreads using the official fold templates
- letter programs should be built against the specific campaign format, not assumed from a generic page size

## Shared Rules For All Poplar Templates

- Start from the official Poplar template for the chosen format whenever available.
- Confirm the campaign format before building creative. In Poplar, campaign setup and template design are tightly coupled.
- Keep all linked assets final-quality and stable.
- Render a PDF proof for every new template version before activation.
- Map merge-tag names directly to your source-data keys and freeze those names early.

## Static Upload Workflow

Use static uploads when the mailpiece is designed fully outside Poplar.

Recommended workflow:
1. Pick the exact format first.
2. Download or recreate the official Poplar template frame for that format.
3. Design to the upload dimensions, not just the trimmed dimensions.
4. Export final art at the required resolution.
5. Upload and review the rendered proof.
6. Validate address-block behavior on the mail-facing side.

### Shared static-upload checklist

- Use the official upload size, not only the final trim size.
- Keep critical content inside the safe zone.
- Avoid placing copy across folds unless the fold behavior is intentional.
- Treat the mail-facing panel as the primary attention surface.
- Review the final proof for address overlap, panel alignment, and bleed.

## Dynamic HTML and CSS Workflow

Use dynamic HTML and CSS when the mailpiece needs deeper personalization or reusable layout logic.

Poplar's dynamic-template guidance emphasizes:
- absolute positioning for placed elements
- inline CSS or a style block in the document head
- inches or pixel units
- publicly accessible image URLs
- explicit font handling

The Poplar dynamic docs also describe liquid-style templating concepts for advanced personalization workflows. Treat complex personalization as a rendering system, not as a last-minute copy merge.

### Dynamic-template build checklist

- Match template fields to stable merge-tag names.
- Keep layout logic simple before adding loops or conditionals.
- Use a fallback value for any field that might be missing.
- Proof edge cases, not only happy-path data.
- Verify line breaks, overflow, and image-fit behavior in the rendered PDF.

## Format-Specific Guidance

## 4x6 postcard

Static upload dimensions from Poplar's guide:
- upload art: 1275 by 1875 pixels
- trimmed art: 1200 by 1800 pixels
- safe zone: 1050 by 1650 pixels

Recommended use:
- high-volume offers
- simple win-back or retention
- strong one-off CTA

Build notes:
- front should carry the main stopping power
- back should reserve space for address and postal information
- keep CTA and offer cluster away from the address zone

## 6x9 postcard

Static upload dimensions from Poplar's guide:
- upload art: 1875 by 2775 pixels
- trimmed art: 1800 by 2700 pixels
- safe zone: 1650 by 2550 pixels

Recommended use:
- richer headline plus benefits
- more generous offer box
- slightly denser storytelling than 4x6

Build notes:
- this is often the most flexible postcard format
- front can support a hero plus subhead
- back can support short body copy, QR, URL, offer, and still remain readable

## 6x11 postcard

Static upload dimensions from Poplar's guide:
- upload art: 1875 by 3375 pixels
- trimmed art: 1800 by 3300 pixels
- safe zone: 1650 by 3150 pixels

Recommended use:
- premium prospecting
- larger assortments
- more visual merchandising

Build notes:
- treat this as a premium canvas, not just a larger postcard
- use the extra space for hierarchy, not clutter
- confirm the mail-facing side still protects the address zone

## 8.5x11 bifold long

Poplar's static guide lists this format with a dedicated upload template. Use the official fold template as the source of truth for panel order and fold behavior.

Recommended use:
- more storytelling than a postcard
- multiple product or service sections
- formats that need an inside reveal

Build notes:
- design the entire spread first, then review each visible panel in folded state
- do not place critical text across fold lines
- treat the outer panel as the mailbox-facing stop signal
- keep the inner spread focused on proof, benefits, and CTA sequence

## 17x5.5 bifold short

Poplar's static guide lists this as a separate bifold layout with its own upload template.

Recommended use:
- wide visual storytelling
- side-by-side comparison or before-and-after concepts
- compact but more premium mail than a postcard

Build notes:
- be careful with fold-aware panel order
- avoid compressing body copy into short wide columns without proofing readability
- keep the response mechanism visible without forcing a full unfold

## 16.625x8.75 trifold

Poplar's guide provides a dedicated trifold upload template and art dimensions.

Recommended use:
- catalog-like messaging
- step-by-step narrative or offer progression
- content that benefits from panel sequencing

Build notes:
- panel order is the main risk; proof the folded reading path
- do not assume equal attention across all panels
- reserve one outer panel for the primary hook
- keep the CTA on a panel that will definitely be seen after the first open

## 8.5x11 letter

Poplar's static guide lists a letter format with its own upload dimensions.

Recommended use:
- longer persuasive copy
- trust-heavy categories
- win-back or onboarding that benefits from a personal tone

Build notes:
- design for readability first
- protect white space and paragraph rhythm
- keep the offer and CTA visually distinct from the body copy
- proof variable-length personalized fields carefully

## Practical Guidance By Format Type

### Postcards

- Optimize front for mailbox stop power.
- Put the complete offer on the back in a scannable cluster.
- Keep QR, URL, and promo code close together.
- Validate address-block safety on the back.

### Folded mailers

- Think in panels, not pages.
- Validate both spread view and folded view.
- Keep the first-visible panel strong enough to earn the open.
- Avoid key copy, coupons, or QR codes on or near fold stress areas.

### Letters

- Treat the page like a persuasive document, not a poster.
- Prioritize typography, spacing, and offer clarity.
- Test the proof with the longest realistic personalized fields.

## Proofing And QA

For every Poplar template:
- render a PDF proof
- test missing and long-field fallbacks
- verify images resolve from production-safe URLs
- confirm fonts render as expected
- check for clipping, overflow, and panel misalignment
- confirm the piece still works if a recipient only scans the first visible side

## Using The Starter Assets

These starter files are designed to be:
- uploaded directly as dynamic HTML starting points
- copied and adapted to a campaign-specific creative
- used as reference structure for panel order and merge-tag usage

Before using them live:
- verify the selected campaign format matches the asset filename
- confirm all custom merge tags exist in your CSV or API payload
- review the PDF proof with longest-value and blank-value test records
- remove or restyle any placeholder brand text

## Common Failure Modes

- building to trim size instead of upload size
- forgetting the address or postage zone
- treating folded pieces like flat pages
- relying on untested dynamic fields
- using unstable or rate-limited asset hosting
- approving a template without checking the rendered PDF
