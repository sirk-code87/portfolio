# Build log

Reconstructed from my own dated notes.

### 2026-09  diagnose failing connected-account creation

Dashboard creates returned errors but still made accounts. I traced the fault to the follow-up list call, which failed at larger page sizes, and found that retries either replayed the error or made duplicates.

### 2026-10  spec and build the incident create tool

Standalone app, one restricted key, a create-once flow through fixed steps (a state machine) with an "unconfirmed" stop, stable signed onboarding links, and an audit log that stops any action whose record cannot be written. Built in a day by Claude Code agents in waves, each checked by a reviewer agent.

### 2026-10  rank account-ops opportunities and decide to grow the tool

Agent-assisted research across support tickets, Slack and transcripts produced 13 opportunities, scored with a stated formula. I chose to grow the tool into an ops console, reads first.

### 2026-10  ship six read-only slices

Registry, verification queue and chase log, property links, risk queue, payout panels and hygiene. All six only read from Stripe.

### 2026-10  spec the update link with a STRIDE threat model

STRIDE is a standard checklist of six threat types. Chose one of six write options and put the rest on a Not Doing list. Shipped with live use switched off behind a staged risk acceptance.

### 2026-10  user testing with implementations and same-day fixes

Fixed the create form around how the team actually worked: optional property reference, account name in place of property name, merchant category left to the property.

### 2026-10  install the live key and sync the registry

Live sync of every connected account. Shrank list pages after the list endpoint failed at larger sizes in live use. I waived my own pre-live security review here.

### 2026-10  merge to one restricted key and arm the live switch

Separate keys in one deployment gave separation on paper only. Charges read stayed off the key.

### 2026-10  rename, roles and a work-needing-action home page

Viewer, operator and admin roles, with an admin-granted permission to see amounts.

### 2026-10  feedback through the insight coach, privacy pack

A "Give feedback" modal routes requests to Productboard through the insight relay. Public privacy notice, IP shortening and a privacy screening.

### 2026-10  reframe dispute alerts around the property's response

Staff were leaving dispute decisions unlogged, so alerts now ask whether the property has responded.

### 2026-10  restore service after a database outage

The console went down silently, because alerts depended on the same database. Lesson: monitoring must sit outside the app.

### 2026-10  CRM account picker and Stripe Dashboard deep links

Staff pick a CRM account or skip with a logged reason. Stripe IDs open in the Dashboard.

### 2026-11  keep the console permanently

Decided on my own assessment of account creation. Recorded in ADR 0008 as a gate I designed and then set aside.

### 2026-11  separate development from production data

Found development sharing the production database and moved development onto a copy.

### 2026-11  capture finance contacts at property creation

Contact capture, with contacts deleted for properties that never complete setup and kept out of audit rows and alerts.
