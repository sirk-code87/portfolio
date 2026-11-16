# 0007. One narrow restricted key, never Charges read

**Date:** 2026-10

## Context

The incident tool started with a restricted key (a Stripe key limited to named permissions) that could create accounts and account links. The ops slices needed reads: accounts, disputes, fraud warnings, reviews, payouts and balances. The risk queue also needed each dispute's order reference to link it back to a booking, and that reference was only on the payment object.

## Decision

After a period with separate read and write keys, I merged back to one restricted key in one deployment, scoped to the permissions the features use. It includes a narrow payment read for order references, and the order reference is the only field stored from that object. Charges read (access to Stripe's record of each card payment) stayed off the key throughout.

## Alternatives rejected

- **Separate read and write keys in the same deployment.** Anything that could read one could read the other, so the separation was mostly on paper.
- **Charges read to find the merchant on a dispute.** Charges carry cardholder data, and the console needed none of it. The registry's property links answer the same question.
- **Map orders through a data warehouse export.** It covered one booking system only and arrived a day stale.

## Consequences

- One secret to rotate and reason about.
- A compromise of the deployment exposes both read and write, which the live switch and the narrow gateway ports (small interfaces that expose only the calls each feature needs) partly offset.
