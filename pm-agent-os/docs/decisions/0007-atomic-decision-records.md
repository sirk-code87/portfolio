# 0007: Atomic decision records with a door type and a named decider

## Context

Decisions lived in one decision-log file per project, and risks in one register per project. Neither could be searched across projects, and an agent reading a log could not tell an agreed decision from a proposal or an opinion. Agent drafts made this worse: a confident draft reads like a decision.

## Decision

I made each decision, risk and open question its own note, with a fixed set of properties at the top of the note.

- Every decision names who decided, the date, and its door type: one-way if it is hard to undo, two-way if it is easy. The door type sets how much evidence it needs.
- Claims elsewhere are classed as decided (with a citation to the record), proposed, opinion or unknown; evidence is labelled as evidence, inference or assumption.
- Persona input is recorded separately from the decider, so "three delegates drafted this" never reads as "three people agreed".
- The session log writes two-way decisions at the moment I make them.
- Each project hub embeds views of its own decisions, risks and questions.

## Alternatives rejected

- **Keep per-project log files and add tags.** Still one file per project, and still free text.
- **Let agents mark decisions as agreed when the conversation implies it.** That is how a draft becomes a fact.

## Consequences

- 1,177 decision records, 106 risk records and 65 question records by handover.
- Agents could proceed on two-way doors by marking a default as proposed, and stopped on one-way doors (writes to systems of record, secrets, merges, deploys) until I authorised them.
- More files to keep valid. A consolidation pass on note properties found the emitters (the scripts that log events) had drifted from the standard; afterwards 3,050 files validated with zero warnings.

## Date

August 2026 (risk notes, question notes, decision notes); session-time capture September 2026
