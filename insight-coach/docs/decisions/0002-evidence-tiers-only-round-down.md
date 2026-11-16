# 0002. Evidence is tiered as quote, observation or inference, and doubt rounds it down

## Context

A product manager needed to tell a customer's own words from a colleague's guess, and once the coach had framed them, both read the same. The AI model also produced tier labels outside the allowed set. An early version rejected those drafts outright, so a good insight with an odd label failed to file.

## Decision

- Three tiers: quote (the customer's words), observation (the raiser saw it), inference (the raiser's read).
- The filed tier is the lower of the model's proposal and a cap applied in code.
- Known synonyms map to their tier. Anything unrecognised becomes inference.
- The raiser can lower the tier on the card.

## Alternatives considered

- A free-text evidence field: impossible to sort, and it invites "strong evidence" with nothing behind it.
- Rejecting drafts with unknown labels, as the first version did: loses insights over a formatting slip.

## Consequences

- The mapping always errs low: it can under-state evidence and never inflate it. A product manager occasionally asks a follow-up about an insight that deserved a higher tier, which is the cheaper error.
- The original relied on the model to tell a quote from a paraphrase. [Coaching design](../coaching-design.md) describes the stricter check, that a quote's words appear in the raiser's text.
- Tiering measures evidence strength. Actionability is a separate measure, listed as open in [evaluation](../evaluation.md).

## Date

2026-07
