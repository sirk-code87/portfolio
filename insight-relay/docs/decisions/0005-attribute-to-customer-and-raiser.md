# 0005. Attribute each note to its customer and to the person who raised it

## Context

Notes filed by colleagues were linked to the colleague, so the customer field in Productboard, the roadmap tool, named the filer. Demand could not be totalled per customer, and account managers could not see what their customer had raised. A plain-text "submitted by" line in the note body also put personal data where it was hard to erase.

## Decision

- The note's customer is the company the feedback came from. In the insight coach, the chat assistant staff file through, the colleague confirms the company on a card before filing; elsewhere the relay matches it.
- An observation about several customers becomes one note per customer.
- The person who raised it is a Productboard user and the note's creator. The owner is the product manager (PM) for that area.
- Filing never waits on matching. A note with no match is filed without a company and flagged.
- Company matching accepts only words that identify exactly one company, never loose name matching.

## Alternatives rejected

- Linking staff notes to the company's own account: it keeps the original problem.
- Telling the submitter after filing that the company was missing: they know the answer at filing time, so ask then.
- Loose name matching: a wrong attachment is worse than none.

## Consequences

- I updated every existing note with a clear customer to the new model, with no failures, and left the unclear remainder to the area PMs.
- Matching needed the full company list on every call, and loading it from cold took about 12 seconds. I added a shared cache that serves the stored copy while refreshing it in the background, and falls back to fetching directly if the cache fails. That brought the lookup to 0.5 seconds without new credentials.
- Moving the person out of the note body still leaves them in Productboard, now in one record that can be deleted in one call.

## Date

2026-10, amended later that month to set creator and owner as above.
