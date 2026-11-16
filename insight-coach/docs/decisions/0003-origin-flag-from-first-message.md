# 0003. How an insight arrived is judged from the first message only

## Context

The coach's job is to turn requests into problem statements, so every filed insight reads problem-first. That hides whether colleagues were learning to frame problems themselves. Requests polished by a general AI assistant also read well on the surface whatever their starting point.

## Decision

- Each insight carries an origin flag: solution, problem or too terse.
- The flag is set from the raiser's first message only, before any coaching.
- If the coach cannot tell, the flag is left out.
- The flag travels to Productboard as a tag, so product managers can filter by it.

## Alternatives considered

- Classifying the final insight: always comes out as "problem", so it measures the coach.
- Always setting a value: a guessed flag corrupts the one measure of whether framing changes over time.

## Consequences

- The flag is polish-proof. An AI-tidied request that names a fix still counts as a solution.
- An origin value the relay did not expect once stopped insights filing until the relay accepted it. The relay later made every tag removable, so a refused tag is dropped and the note still files; its [0006](../../../insight-relay/docs/decisions/0006-never-lose-a-note.md) records that rule.
- The data to compare framing against the old baseline is in Productboard, and the comparison is still to run.

## Date

2026-07
