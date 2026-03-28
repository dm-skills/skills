# Measurement Reference

Use this file when the main measurement workflow needs deeper economics or benchmarking detail.

## ROI Models

### Raw versus incremental ROAS

- Raw ROAS shows attributed revenue divided by campaign cost.
- Incremental ROAS uses only the lift above holdout.
- Default to incremental ROAS for decisions.

### Incremental CPA

Use:

```text
Incremental CPA = total campaign cost / incremental new customers
```

Use target ranges that fit the business model, margin structure, and expected LTV.

### LTV-adjusted economics

Use LTV-adjusted ROAS for acquisition programs where first-purchase economics understate value. Use retention or win-back economics more conservatively.

### Contribution-margin economics

If COGS or fulfillment costs are meaningful, convert incremental revenue into contribution margin before recommending scale.

## Learning Frameworks

### Learning agenda template

```text
Campaign
- Primary learning question
- Secondary learning question
- Measurement method
- Minimum detectable effect
- Decision rule
```

### Test matrix

Track ongoing winners and challengers across:
- offer
- format
- list source
- creative concept
- segment logic

### Replication warning

Extraordinary first-test results often soften on replication. Re-test before making a major scale commitment.

## Benchmarks

Treat all benchmarks as directional unless you have your own holdout history.

### Response-rate perspective

- Postcards and self-mailers often show lower true incremental lift than raw reported response rates suggest.
- Letters often read better than postcards in raw reporting, but lift still depends heavily on audience quality and offer.
- Win-back and high-intent house-file programs often outperform acquisition.

### Economics perspective

- Retention programs often clear stronger ROAS than acquisition.
- Triggered mail carries higher unit cost but can justify it with stronger intent.
- B2B and premium packages should be judged against deal economics, not consumer-mail response norms.

### Seasonality

Performance shifts by category and calendar. Plan around:
- postal congestion periods
- category demand peaks
- budget cycles
- store or promotional calendars

## Triggered-Mail Measurement

### Design rules

- Split traffic or eligibility before the trigger fires
- Keep holdout persistent for the duration of the test
- Use causal holdout reads for decisions and lighter operational reads for weekly monitoring

### Reporting cadence

- Weekly: volume, failures, early signal
- Thirty-day: first stable lift read
- Sixty-day: trend and fatigue review
- Ninety-day: program decision review

## Common Measurement Mistakes

- Declaring winners before the response curve matures
- Using last-touch attribution as causal proof
- Comparing direct-mail metrics directly to email metrics
- Ignoring undeliverables or actual mailed counts
- Polluting holdout comparisons with poor segmentation logic
- Truncating long-tail response windows
- Testing only the campaigns you already believe in
