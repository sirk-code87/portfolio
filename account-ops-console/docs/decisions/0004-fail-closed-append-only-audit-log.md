# 0004. A fail-closed, append-only audit log

**Date:** 2026-10

## Context

The console creates accounts and sends links on behalf of merchants. If anything went wrong, I needed to answer who did what, when, and what Stripe said, from a record that outlasts hosting logs.

## Decision

Every action that changes data writes an audit entry in the same database transaction (a single all-or-nothing step) as the change. For Stripe writes, an "attempt" entry is written before the call and an outcome entry after it, with the Stripe request ID. The log fails closed: if the audit write fails, the action fails. The log is append-only: the app's database login can only add audit rows, and a database trigger (a rule that runs on every change) refuses edits and deletions from any login.

## Alternatives rejected

- **Log to the hosting platform's logs.** They expire, and they sit apart from the data, so the two cannot be queried together.
- **Best-effort audit after the action.** A failed audit write would leave an action with no record.

## Consequences

- The audit log doubles as the console's measurement source, in place of product analytics.
- Anything written to it stays there. Personal data must be kept out of audit rows, and the privacy approach in [threat-model.md](../threat-model.md) treats that as a release condition.
- A failed audit write blocks real work. I accepted that.
