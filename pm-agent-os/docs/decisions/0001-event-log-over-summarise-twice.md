# 0001: Build the weekly report from an event log

## Context

The weekly report to the leadership team was drafted by agents that scanned daily notes, meeting notes and email, summarised them, and then summarised those summaries into the report. When I audited a run, I found figures that appeared in no source, two topics merged into one, items from earlier weeks reported as new, and verbs that grew at each step ("discussed" became "agreed", "agreed" became "launched"). Each summarising pass lost the link to the source and added confidence.

## Decision

I chose to capture facts once, at source, and build the report only from those facts.

- Emitters (small scripts attached to the session log, decision records, task changes, git and Productboard, the product roadmap tool) add short factual events to one log, and events are only ever added. The agreed fields for an event are versioned and were frozen at version 1.
- Each event's summary verb is a ceiling: the report may describe it no more strongly.
- A two-part significance gate keeps an event only if it records a real state change and passes a business test. It leans towards leaving things out.
- A checker that starts fresh, with no sight of the drafting, compares the draft against the source events and removes anything without one.
- A watermark records the last reported point, so past weeks cannot resurface.
- After the first live run, a source gate: only updates I dictated may headline, because mechanical events had dominated the draft.

## Alternatives rejected

- **Tighter prompts on the existing pipeline.** The fault was structural: any summary of a summary loses the link to the source.
- **A checker alone on the old pipeline.** It would have had nothing reliable to check against.
- **Keeping the daily-note and email scans as extra inputs.** They reintroduced unstructured text; I dropped them and the five scanning subagents.

## Consequences

- Built and tested in under a week: 32 capture tests and 16 report-side tests. It became the live path for the report, and later reports show the checker cutting unlogged figures.
- 4,470 events logged by handover.
- Anything not logged is invisible to the report. That pushed me to make decision and session logging habitual, and meant a quiet week produces a short report.
- I kept a throwaway stopgap for the weeks between the old pipeline and cut-over.
- Every later pipeline (call insights, CRM sync, overnight runner) started from a log or a ledger.

## Date

July 2026
