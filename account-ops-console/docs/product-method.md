# Product method

The console started as an incident fix. Within a day I decided to grow it into an operations tool, despite its temporary status, because account management turned out to be a steady support load that needed a tool of its own. This document covers how I decided what to build, what to leave out, and how I planned to measure whether it worked.

In short, the method ran as designed up to the point of measurement, then stopped. Each section says how far its step got.

## 1. Evidence

I ran agent-assisted research across a year of support tickets, Slack threads, meeting transcripts and Productboard notes. The findings that shaped the ranking were qualitative:

- Most account-related support tickets were disputes and payment reviews.
- Verification tickets followed a template and took several chases over weeks.
- Mapping a property to its Stripe account was unreliable, especially within hotel groups where several sites had near-identical account names.
- Disputes were often lost because evidence missed the deadline.

I also listed the claims I set aside, where two sources measured the same thing on different dates with different definitions.

The benefit claim I set early was "fewer missed deadlines and less chasing". I kept Stripe steps out of the claim, because the property still has to act in Stripe itself.

## 2. Opportunity ranking

The research, with duplicates merged, came to 13 opportunities. I scored each as:

> Score = Reach × Impact × Confidence × Strategic multiplier ÷ Effort

The scores are my judgement and were labelled that way. Their job is to make the reasoning open to challenge.

| Rank | Opportunity | Score |
|---|---|---|
| 1 | Verification queue, ranked by deadline | 10.0 |
| 2 | Account registry | 8.1 |
| 3 | Property-account links | 5.6 |
| 3 | Chase log | 5.6 |
| 5 | Dispute and review risk queue | 3.9 |
| | Payouts, hygiene, payment methods, migration | between |
| | Onboarding update link (first write) | 1.5 |
| | Reject or delete an account | 0.5 |
| | Change account settings | 0.45 |

Reads ranked high because they had wide reach and left the company's liability where it was. Writes ranked low because each one moved risk onto the company or the property.

## 3. Read-only features first

Six slices (small features shipped one at a time), estimated at about 32 builder-days, all reading from Stripe only:

1. Registry: every connected account, sorted by type and current state.
2. Verification queue and chase log.
3. Property-account links.
4. Dispute, early fraud warning and review queue.
5. Payout, balance and payment-method panels, fetched on demand.
6. Hygiene: accounts that never finished onboarding, duplicates, mismatched settings.

Each slice went through the same loop: a builder agent, a reviewer agent checking for security issues, fixes, then merge and deploy. The reviewer agents kept finding defects the builders' tests had missed, which is why that step stayed.

## 4. Six write options, one chosen

| Option | Write | Outcome |
|---|---|---|
| W1 | Onboarding update link for existing accounts | Chosen |
| W2 | Submit or accept dispute evidence | Not doing |
| W3 | Reject or delete an account | Not doing |
| W4 | Change account settings | Not doing |
| W5 | Deauthorise a legacy account | Not doing |
| W6 | Payout actions | Not doing |

I chose W1 because it could do the least harm if misused and reused the create flow's controls: the stable signed URL, tokens stored in hashed (one-way scrambled) form that expire and can be revoked, the audit log and the live switch. I specced it with its own threat model ([threat-model.md](threat-model.md)) and a staged rollout: one consenting account, then a time-boxed pilot, then wider use behind a further gate. I designed the risk acceptance so that I proposed it and others signed it.

W1 shipped with live use switched off, waiting on that sign-off. I did not secure it, so W1 stayed off.

## 5. Not Doing

Written down with a reason each, so the list could be challenged:

- **W3 to W6.** Each waits for evidence of need, and deauthorising an account can break payments already in flight.
- **W2, custom dispute evidence.** Stripe's embedded dispute components may cover the need, so I commissioned a view-only spike (a short trial build) to find out first.
- **Update links on the legacy account rail.** These wait for a live check that confirms the behaviour there.
- **Ownership-change and migration workflow.** The highest-risk write in the set.
- **Embedding in the property-facing system.** This waits for a baseline showing the property is the bottleneck.
- **Charges read.** A narrower read for order references covers the need and keeps cardholder data out of the console.
- **Staff acting on disputes on a property's behalf.** This requires explicit authority from the property first.
- **Refunds, terminals, reconciliation, fee reporting.** Other teams own them.
- **Any promise that the tool removes the property's own Stripe steps.** The property still acts in Stripe itself.

## 6. Hypotheses and measures

The console's own tables served as its analytics. Every measure was designed to come from the registry, account changes, the chase log, risk-case notes and the audit log.

| | Hypothesis | Measure | If wrong |
|---|---|---|---|
| H1 | A ranked verification queue with a chase log cuts missed deadlines | Share of accounts going past due with no chase in the prior 14 days | The tool is a viewer only; cut the chase log |
| H2 | Disputes lapse mainly because the property does not reply | Share of lapsed cases marked "no reply from property" | Decides between a property-facing tool and staff-side handling |
| H3 | Update links clear requirements faster than chasing alone | Median days to clear, and share cleared within 14 and 30 days, against a matched chased-only group | Stop W1 at the pilot |

Supporting measures per slice: open queue by state, days from restricted to cleared, chases per cleared account, share of active accounts with a property link, and a mismatch gate of under 10% on a 20-row spot check. I stated in advance that the W1 comparison was observational: a signal, not proof.

H2 started with low confidence, and H3 had limited reach because update links applied to one of the two account rails.

**What happened.** All three remain untested. I did not run the baseline queries, W1 stayed switched off, and staff left dispute decisions unrecorded, so the count of undecided disputes kept growing. That last signal was useful. It showed that asking staff to log a decision was the wrong ask, so I reframed dispute alerts around whether the property had responded.

## 7. The keep, widen or retire gate

1. The incident tool had a removal date from the start, triggered once Stripe was fixed and Dashboard creates had worked for two weeks.
2. When I grew the tool, the review became keep, cut back or retire, to be decided on baseline, user-testing and pilot evidence, with no new feature slice before then.
3. My agent team proposed a feature freeze. I left it unrecorded as a decision, and I exempted three pieces of work within a week.
4. I then decided to keep the console permanently on the strength of account creation, and cancelled the review. The decision rested on my assessment, without a usage count or survey.

The gate was the right design, and I set it aside. At the point I decided, the evidence was one live property created through the console, qualitative feedback from two teams, and a registry people used without anyone counting how often. A disciplined version would have kept the registry and queues, recorded the create flow as an incident tool whose incident had ended, and set a date to read the baseline before widening anything.
