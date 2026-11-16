# 0008. Keep the console as a permanent tool

**Date:** 2026-11

## Context

The console had a keep, cut back or retire review scheduled, to be decided on baseline, user-testing and pilot evidence. The evidence at that point: the implementations team was using it, two teams had given positive feedback, and one live property had been created through the console. The baseline queries and the update-link pilot were still to come.

## Decision

I decided to keep the console permanently on the strength of account creation and cancelled the scheduled review. Other functions might later sit behind feature flags (switches that turn a feature on or off per user or team); that was left open.

## Alternatives rejected

- **Retire it at the review date.** The registry and queues were in use, and retiring them would put staff back on spreadsheets and the Dashboard.
- **Keep everything with no flags.** Left open.

## Consequences

- The console has a long-term owner and a future.
- The decision rested on my assessment, without a usage count or survey. The evidence I held supported keeping the registry and queues; it gave little support to the reason I gave, account creation. I record this decision as an example of a gate I designed and then set aside. See [product-method.md](../product-method.md), section 7.
