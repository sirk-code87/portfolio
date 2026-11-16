# 0002. Never retry account creation automatically; block on unconfirmed

**Date:** 2026-10

## Context

During the incident, the create call succeeded but the response often came back as an error. Stripe uses an idempotency key (a tag that marks two requests as the same request) to prevent duplicates. A retry with a new key created a second account; a retry with the same key replayed the stored error. Every retry made the set of accounts messier.

## Decision

The console calls create once. Any error that leaves the outcome unclear (server errors, timeouts, connection failures, rate limits, conflicts) is treated as "probably created". The console then looks for the account with a small list call, a few times with a growing pause between tries, matching on metadata it set. If it cannot find it, the merchant moves to `unconfirmed` and creates for that merchant stay blocked until an admin attaches the right account. Only a definite refusal from Stripe lets an admin reset the merchant and try again.

## Alternatives rejected

- **Let operators retry with a fresh key.** That is how the orphaned accounts were made.
- **Retry quietly in the background.** It hides the problem and still risks duplicates.
- **A written "never re-register" rule.** It relies on people under pressure remembering it.

## Consequences

- The console creates each merchant's account exactly once, at the cost of occasional manual admin work.
- The step-by-step create flow (a state machine) in [architecture.md](../architecture.md) follows from this decision.
