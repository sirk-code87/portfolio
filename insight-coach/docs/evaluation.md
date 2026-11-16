# Evaluation

The coach's behaviour lived in its prompts (the written instructions given to the AI model), so a prompt edit was a product change. This document covers how I meant to gate those changes with evaluations (automated tests of the coach's answers), how the gate eroded, and the design I'd use so any erosion shows. It ends with what was measured in use and what is still open.

## The gate as designed

The rule in the original repository was simple: the evaluation suite passes before any prompt change merges.

The suite had two kinds of fixture, meaning a fixed test case with a known expected result:

- **Coaching fixtures**: a scripted opening message and the behaviour expected of the coach. Does it ask for the problem behind a solution-first request? Does it stop at three questions? Does it offer to file as-is?
- **Extraction fixtures**: a finished conversation and the insight it should produce. Is the title problem-first? Is the tier right? Does every specific in the problem statement come from the raiser?

Checks fell into two classes.

| Class | What it checks | How |
|---|---|---|
| Rule-based | Data shape, allowed values, tier at or below the cap, verbatim text unchanged, origin flag present or honestly absent, question count | Code checks on the output |
| Judged | Problem-first framing, every number and cause sourced from the raiser, the problem stated as a need, tone | A second AI model from a different provider (the judge), scoring each case against a written rubric |

The judge took a majority of three runs per case, stopping early once the outcome was settled, with a limited number of retries for temporary errors. Most flaky results came from the gateway's free tier limiting parallel calls, so cases ran one at a time. At one point the full test suite, unit tests and evaluations together, stood at 365 passing tests.

## How the gate eroded

Each step below was my decision.

- **Waived under delivery pressure.** I let a prompt change ship without a full evaluation run.
- **Skipped for a vocabulary change.** When I replaced free-text problem themes with a fixed list ([0007](decisions/0007-fixed-problem-themes.md)), I changed the prompt and skipped the evaluations.
- **Dropped as a launch gate.** The external variant of the coach needed judged evaluations before launch. The gateway's free tier refused the judge model, and I chose to stay on the free tier. I first merged on the condition that the internal prompts were byte-identical to the evaluated ones, which a hash check (a comparison of file fingerprints) confirmed. I then removed judged evaluations as a launch condition altogether. The decision record and the spec still named them as a gate.
- **Green by skipping.** The CI job (the automated checks that run on every change) had no API key. Every evaluation that needed the model skipped, and a run with skips looked the same as a run that passed.

The pattern is the same each time. The gate depended on someone choosing to run it, and skipping it cost nothing anyone could see.

## A gate that is hard to erode

The design I'd use now, with the reasoning for each part.

1. **Golden cases that name the failure modes.** About twenty reference cases covering solution-first requests, terse one-liners, a pasted transcript, an inflated evidence claim, two problems in one message, and an immediate file-as-is.
2. **A rule-based tier on every pull request, with no key.** A pull request is a proposed change awaiting review. Recorded model outputs are replayed through the same validation code the server runs. This catches changes to the data shape, inflated tiers and altered verbatim text at no cost, and it always runs because it needs no credentials.
3. **A judged tier on prompt changes.** The same cases run live, plus the judged rubrics, with a majority of three. A judge from the same provider as the model under test is weaker than one from another provider, and the results should say which was used.
4. **Hash-locked prompts.** CI takes a fingerprint of every prompt file. The build passes only when the latest stored evaluation result carries that fingerprint and a pass, so a prompt edit merges only with a fresh passing run.
5. **Skip counts as fail.** A missing key on the live job is an error. Any skipped test fails the build.
6. **Required checks, logged waivers.** Branch protection (a repository setting that blocks merging until named checks pass) makes the evaluation job required. Any waiver is written into the build log with its reason, so erosion is visible in the history.

Parts 4 and 5 address the actual failure. Hash locking ties a prompt edit to evidence that it was evaluated, and treating a skip as a failure removes the quiet green.

## What was measured in use

| Measure | Result |
|---|---|
| Submitters, week before launch against two weeks after | 2 to 19 |
| Chats started that ended in a filed insight, first six weeks | 81 of 92, about 88% |
| Drafts shown on the card that were filed | 81 of 82 |
| Insights per day from the coach, end of first month | about 6 |
| Book-a-chat clicks from the completion screen | 0 in the first week, 11 by the end of the second month |

## What was not measured

- **Problem-first rate against the baseline.** The pilot's headline measure was the share of insights that open with the problem, compared with about 9% in the old intake. What would settle it: scoring a sample of filed insights against that baseline.
- **Actionability.** The pilot plan named a product-manager rating of whether each insight could be acted on as the decision metric. A review of the pilot found the app tiered evidence and left actionability ungraded, and the rating was never collected.
- **Effect on prioritisation.** Whether coached insights changed what the product team built is untested.
- **Judged evaluations for the external variant.** Its prompts went unjudged before the integration was handed over.

The funnel numbers show that people who started a chat finished it and filed. Quality, the reason for building a coach, needs the measures in this list.
