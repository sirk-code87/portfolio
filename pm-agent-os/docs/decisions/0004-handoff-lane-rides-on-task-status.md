# 0004: The handoff lane rides on task status

## Context

Delegable work (research, first drafts, analysis) sat on my task board waiting for me. My first design for handing it to agents added an `agent-handoff` tag, a block of handoff properties at the top of each task note, a ledger, a dedicated folder and a separate review task for each handoff.

## Decision

I judged the first design too heavy and replaced it with two new task statuses, Delegate and Review, on the existing kanban (a board of cards in columns).

- Dragging a card to Delegate is the whole handoff.
- A scheduled job on my laptop runs the handoff command three times each weekday. It takes at most three tasks per run, routes each through the `/team` dispatch table to one to three delegates with one primary, and holds a lock so runs cannot overlap.
- Delegates are draft-only. The primary's combined draft is written into the task under a delegate-output heading.
- The task moves to Done or Review. From October, Review holds only decisions I must make; outbound messages become drafts in Slack or email.
- The board itself is the record: a card's column shows where the task stands.

## Alternatives rejected

- **The tag, properties, ledger and folder design.** Five moving parts to answer "is this with an agent?", which a status answers alone.
- **A separate review task per handoff.** It doubled the board.

## Consequences

- Built and running from September 2026 on the existing storage.
- Dropping the ledger left no trust data. Not yet measured: the share of drafts I accepted.
- The one throughput signal is a backlog: in October 57 tasks sat in Review, mixing finished work, messages and decisions, and only 22 carried the "to finish" note the design specified. That forced the narrowing of Review to decisions only.

## Date

September 2026; Review narrowed October 2026
