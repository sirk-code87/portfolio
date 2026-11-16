# 0004. Nothing files without confirmation, and the raiser's own words travel unchanged

## Context

A coach that writes on someone's behalf can put words in their mouth. If a colleague found their name on an insight that overstated what they said, they would stop using the tool, and a product manager would stop trusting the board. Colleagues also saw product as a black box: feedback went in and they heard nothing about what happened to it.

## Decision

- Before filing, the coach plays back the insight on a card showing every field, including tier, origin flag, customer and theme.
- Filing requires an explicit confirm from the raiser.
- Everything the raiser typed is filed verbatim as its own field. The coach writes only the title and the problem statement.
- The drafts follow the anti-laundering rules: every number, cause and customer comes from the raiser, and the problem statement describes the need behind any requested fix.
- After filing, a completion screen says where the insight goes and that individual replies are rare by design, and offers a way to book time with the product team.

## Alternatives considered

- Filing automatically when the conversation ends: fast, and it removes the raiser's chance to catch a wrong framing.
- Filing only the coach's summary: loses the evidence a product manager needs to check the framing.
- Promising a personal reply to every insight: it cannot scale, and a broken promise costs more trust than an honest one.

## Consequences

- Of the people who reached the card in the first six weeks, all but one filed. The confirm step cost almost nothing in drop-off.
- The promise to raisers fits in one line: what's filed is what you confirmed.
- The relay's AI-judged lanes later took the opposite approach and filed each note without a person approving it. That fits channels where nobody is present to confirm; the coach kept its card. See the relay's [0003](../../../insight-relay/docs/decisions/0003-judged-lanes-need-no-approval.md).

## Date

2026-07
