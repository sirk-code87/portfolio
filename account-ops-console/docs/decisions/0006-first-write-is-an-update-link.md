# 0006. The first new Stripe write is an update link for existing accounts

**Date:** 2026-10

## Context

Six write options were on the table: update links for existing accounts, dispute evidence, rejecting or deleting accounts, changing account settings, deauthorising legacy accounts, and payout actions. Each moves risk onto the company or the merchant.

## Decision

I chose an onboarding update link that support can send to an existing account's contact of record. It reuses the create flow's controls: a stable signed URL, hashed expiring tokens, the audit log and the live switch. It shipped with live use off, behind a staged risk acceptance (a sign-off by named people, one rollout stage at a time) that I proposed and others were to sign.

## Alternatives rejected

- **Embedded Stripe components in the property-facing system.** Larger build, justified only once evidence shows the property is the bottleneck.
- **Dispute evidence submission.** Irreversible, and it needs explicit authority from the merchant first.
- **Stop and validate before any write.** Defensible; I judged a write switched off in live mode cost little to hold ready.

## Consequences

- The other five options went on the Not Doing list with reasons.
- The update link stayed switched off because I did not secure the sign-off. It remains a specified, tested option awaiting a live result.
