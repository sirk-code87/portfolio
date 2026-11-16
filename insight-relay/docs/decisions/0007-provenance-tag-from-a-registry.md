# 0007. Every note carries a provenance tag from a stable registry

## Context

Survey notes were tagged with a code built from the survey's title. When a title changed, the tag changed with it, and Productboard, the roadmap tool, could refuse the new value. A reader of the board also had to open a note to see which form or channel it came from.

## Decision

Every note from every lane carries a provenance tag, naming its source, in the form `origin:<channel>-<code>`. The code comes from a registry, a fixed list keyed by each source's permanent ID, never its title. A source missing from the registry still files, with an `unregistered` code, so the gap shows on the board and the note survives.

## Alternatives rejected

- Codes built from titles: they break on rename.
- Reusing the existing survey tag: it would mix old and new meanings in one field.

## Consequences

- Adding a source means adding one registry entry.
- Filtering by channel and form became possible across the whole board, which showed when one noisy channel was dominating the counts.
- I rolled the tag out across the five lanes running at the time in one change, covered by the shared tests.

## Date

2026-09
