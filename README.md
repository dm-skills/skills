# dm-skills

A comprehensive direct mail skills framework for AI coding assistants.

Built for Claude Code, Cursor, Gemini CLI, Codex CLI, GitHub Copilot, and any
tool that supports the skills standard.

**Plan → Create → Execute → Measure.** Four production-grade skills covering
every phase of a direct mail campaign, plus a master strategist to tie them
together.

---

## Why This Exists

Direct mail is having a renaissance. Physical mail cuts through the inbox noise
that is burying digital marketing, but running a good direct mail program still
requires deep, cross-functional judgment: audience strategy, offer design, copy
and layout, print production, list operations, and real measurement.

Most teams, and most AI assistants, do not naturally have that depth.

These skills are designed to give your assistant credible direct mail judgment,
not surface-level marketing summaries.

---

## Skills Overview

| Skill | Phase | What It Does |
| --- | --- | --- |
| [`dm-plan`](./dm-plan/SKILL.md) | Plan | Campaign architecture, segmentation, offer design, budget modeling, list strategy, timeline planning |
| [`dm-create`](./dm-create/SKILL.md) | Create | Copy frameworks, design direction, format specs, personalization, compliance |
| [`dm-execute`](./dm-execute/SKILL.md) | Execute | Print production, list hygiene, mail house coordination, triggered mail workflows, USPS execution |
| [`dm-measure`](./dm-measure/SKILL.md) | Measure | Incrementality testing, holdout design, attribution methods, ROI modeling, benchmarks |
| [`dm-strategist`](./dm-strategist/SKILL.md) | All phases | Master orchestrator for strategic guidance, workflow routing, and cross-phase decisions |

### The Four Phases

```text
PLAN          CREATE          EXECUTE         MEASURE
────────      ────────        ────────        ────────
Strategy      Copy &          Print &         Holdout
Audience      Design          List            Design
Offer         Formats         Mail            Attribution
Budget        Specs           Delivery        ROI Analysis
              Compliance      Tracking        Learning Loop
```

Measurement is designed during planning, not after the campaign is already in
market.

---

## Install

### Via `npx skills` (recommended)

```bash
npx skills add dm-skills/skills
```

This auto-detects your AI harness and installs to the correct directory.

### Manual installation

Copy the skill directories into your tool's skills folder:

```text
your-project/
├── .claude/skills/
│   ├── dm-plan/
│   ├── dm-create/
│   ├── dm-execute/
│   ├── dm-measure/
│   └── dm-strategist/
├── .cursor/skills/
├── .gemini/skills/
├── .codex/skills/
└── .agents/skills/
```

### Update

```bash
npx skills update
```

---

## Usage

### Start with the orchestrator

For a new campaign, or whenever you are not sure where to start:

```text
Use the dm-strategist skill. I want to launch a direct mail
acquisition campaign for a premium coffee subscription brand.
Budget $25K. Help me plan and execute the full campaign.
```

### Capture your context first

```text
Use the dm-strategist skill and run /dm-teach
```

This captures your business context, goals, budget, and experience level so the
skills can give more specific guidance instead of generic advice.

### Use the phase skills directly

```text
Use the dm-plan skill to design a win-back campaign for customers
who haven't purchased in 6 months. Our average LTV is $280.

Use the dm-create skill to write copy for a 6x9 postcard targeting
lapsed subscribers with a 20% win-back offer.

Use the dm-execute skill to help me set up a triggered postcard
workflow for cart abandoners.

Use the dm-measure skill to design an incrementality test for our
next retention drop. We're mailing 15,000 pieces.
```

---

## Philosophy

These skills are built on three core principles:

**1. Incrementality over attribution theatre**

Promo codes and matchback analysis capture only part of the real effect and can
materially overstate performance. These skills design for holdout-based
measurement from day one.

**2. Physical medium, physical thinking**

Direct mail is not email on paper. Format, stock, finish, weight, timing, and
mailbox competition all shape performance. The skills treat the physical medium
seriously.

**3. Test, learn, compound**

Every drop should teach you something that makes the next one better. The
measurement and strategist flows are designed to help teams compound learnings
rather than run one-off campaigns.

---

## Industry Verticals

The strategist includes dedicated playbooks for:

- **E-Commerce**: win-back, retention, acquisition, list strategy, seasonal timing
- **Financial Services**: FCRA-sensitive workflows, long response windows, cross-sell strategy
- **Non-Profit**: donor acquisition, sustainer conversion, year-end fundraising
- **B2B**: ABM sequencing, dimensional mail, meeting-focused measurement

---

## Website

Learn more at [directmail.coach](https://directmail.coach/).

---

## License

This repository is source-available under the `DM Skills Internal Use License
v1.0`.

In plain English:

- You may use and adapt the skills internally for your own organization's
  direct mail campaigns and operations.
- You may use the outputs commercially, so long as your use of the skills
  themselves complies with the license.
- You may not use the skills to perform direct mail work for clients or other
  third parties.
- You may not copy, repackage, redistribute, white-label, or resell the skills.

See [`LICENSE`](./LICENSE) for the full terms.

---

## Notes

This repository is the public install/distribution repo for the DM Skills
package. It is synced from the canonical source repository.
