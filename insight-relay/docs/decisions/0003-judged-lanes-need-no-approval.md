# 0003. Judged lanes file without per-note approval

## Context

The insight coach, a chat assistant that helps staff write up feedback, asks the person filing to confirm every note. When the in-app feedback channel was being added, the question was whether new channels should copy that confirm step, for example through a review queue a person cleared before filing. The automated channels already running filed without approving each note. Their quality came from the judge's charter (the written rules for what counts) and the dry-run period, a test mode in which a lane reports what it would file and files nothing.

## Decision

New automated channels follow the judged-lane pattern: an AI judge on a schedule, a dry-run period, and a graded summary I review before the lane goes live. Per-note confirmation stays where a person is present at filing time, which is the coach.

## Alternatives rejected

- A review queue for a fixed period after launch: a daily chore with no owner, and it delays the evidence it was meant to protect.
- Copying the coach's confirm flow: nobody is present to confirm a survey response.

## Consequences

- Quality rests on the charter and on the review before launch, so the charter is the asset to protect and keep under version control.
- A judge whose results drift can file poor notes until someone reads a summary. Summaries keep posting after go-live so drift stays visible.

## Date

2026-09
