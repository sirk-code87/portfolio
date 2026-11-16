# 0005. Grow the incident tool into an ops console, reads first

**Date:** 2026-10

## Context

The incident tool already had a database shaped like a registry, a restricted key, sign-in and an audit log. Research across a year of support tickets, Slack and call transcripts showed staff handling verification chases and disputes with no supporting tool and no reliable map from property to account. The tool was meant to be temporary.

## Decision

I grew it into an operations console despite its temporary status, and kept the first phase read-only: six slices (small features shipped one at a time) covering the registry, verification queue and chase log, property links, the risk queue, payout and balance panels, and hygiene. The keep-or-retire review was widened to cover the whole console.

## Alternatives rejected

- **Keep the incident scope and retire on schedule.** It would leave the account work with no tool.
- **Build it inside the spa management system or a platform programme.** It would take months and compete with other priorities.
- **Stop and validate before building anything.** Reasonable, and closer to what I would do now. I judged that cheap read slices would be the validation. In practice I skipped the measurement that would have made them so.

## Consequences

- Six slices shipped within a week, all reading from Stripe only.
- Speed of building outran evidence. See [product-method.md](../product-method.md), section 7.
