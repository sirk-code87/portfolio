# 0001. A coached chat of up to three questions, with filing open at every turn

## Context

Feedback from colleagues arrived solution-first and thin. In a sample of a few hundred change requests from the old intake board, about 9% opened with the problem. Colleagues were busy and had never been shown how to frame a request. Some already used a general AI assistant to tidy their requests, which improved the wording and hid the solution-first starting point.

## Decision

Build a coach that asks questions and drafts the insight.

- Up to three questions: the problem behind the request, the evidence, and frequency or impact if missing. Each is skipped when the answer is already there.
- The raiser can file as-is from the first turn. Every reasonable submission files; only a message too short to read goes back for more.
- The product choice moved from the chat to the card, then out of the standard mode altogether.
- Pasted transcripts and long emails are welcome, and the coach finds the problem in them itself.

## Alternatives considered

- A structured form with required fields: it collects the same solution in more boxes and gives no prompt to think about the problem.
- An AI rewrite of whatever the colleague typed: it produces fluent text that dresses a solution up as a problem.
- Questioning until the insight meets a quality bar: people would abandon it, and a missed insight costs more than a thin one.

## Consequences

- About 88% of people who started a chat filed an insight, so the questions cost little in drop-off.
- Capping questions means some insights file thin, and the evidence tier and origin flag record that openly.
- The effect on problem-first framing is one of the open measures in [evaluation](../evaluation.md).

## Date

2026-07
