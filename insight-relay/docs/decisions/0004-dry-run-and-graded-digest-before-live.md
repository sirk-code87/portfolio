# 0004. Every lane runs dry with a graded digest before it files

## Context

A judged lane, where an AI model decides what to file, can file many notes in one run. A wrong charter (the lane's written rules for what counts), company match or tag would repeat that mistake across the roadmap tool before anyone noticed, and cleaning notes out of Productboard is slow manual work.

## Decision

Each lane has a live switch that defaults to off. With it off, the lane runs in dry-run: it goes end to end, posts a digest (a summary to Slack) of what it would have filed, and files nothing. The switch counts as on only when set explicitly; a missing or malformed value means dry. I grade the dry digests item by item, useful or not and why, and adjust the charter until the grades hold, then switch the lane live. Those grades became the calibration record for the charter.

## Alternatives rejected

- A separate output destination for dry runs: easy to point at the wrong place, and it tests different code from the live run.
- A flag carried in the judge's own output: the model should not decide whether its work is filed.

## Consequences

- New lanes take a week or more to go live. I accepted that.
- The switch doubles as a kill switch for a lane that starts misbehaving.
- Grading showed one lane was precise but missed too much. Only grading item by item revealed that.

## Date

2026-07, made the standard for all new channels 2026-09.
