# 0002. One engine for many channels

## Context

By the time the call-transcript channel was designed, the survey lane already matched notes to companies, posted summaries to Slack and recovered from failures. A new repository per channel would copy that code, and each copy would drift. I wanted one place to change the quality bar, the customer-matching rules and the credentials for Productboard, the roadmap tool.

## Decision

Every new channel joins the existing relay as a lane. A lane supplies three things: how to fetch from its source, the charter its AI judge applies (the written rules for what counts), and its schedule. Everything after judging is shared code: skipping items already filed, removing personal details, tagging the source, matching the customer, filing and posting the summary. The direct lane, the webhook other tools call to file a single note, works the same way. Features such as the optional Slack feed for each tool are built once in the relay, so every caller gets them.

## Alternatives rejected

- A repository per channel: faster to start, and every fix would need repeating.
- Building intake into each tool that wanted a feedback route: two internal tools asked for one, and both chose the shared pipeline.

## Consequences

- A change to shared code can break every lane at once, so the shared code carries most of the tests.
- The relay becomes the single place credentials live. That kept the access model simple and made ownership of those credentials a handover question I had to answer explicitly.

## Date

2026-07
