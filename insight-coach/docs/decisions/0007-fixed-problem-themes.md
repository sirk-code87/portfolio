# 0007. Problem themes come from a fixed list

## Context

The coach originally gave each insight a free-text problem theme so product managers could group related insights. Free text drifted until there was nearly one distinct theme value per note, and Productboard refused to create most of the new tag values the coach generated. Grouping by theme stopped working.

## Decision

- The coach picks one theme from a fixed list of 28.
- Anything outside the list maps to "other".
- The product team owns the list and changes it deliberately.

## Alternatives considered

- Keeping free-text themes: grouping stays broken, and every new value is another tag Productboard may refuse.

## Consequences

- Themes became groupable again, and the theme tag stopped being a filing risk.
- A fixed list folds genuinely new themes into "other". The count of "other" is the signal to revise the list.
- I made this prompt change and skipped the evaluations. [Evaluation](../evaluation.md) covers that as one step in the gate's erosion.

## Date

2026-09
