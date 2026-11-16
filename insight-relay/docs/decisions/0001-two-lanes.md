# 0001. Two lanes: a direct lane for confirmed notes, a scheduled pipeline for judged ones

## Context

The first channel was a Claude Code skill (a packaged command for Anthropic's coding assistant) that let product staff file a note without a login to Productboard, the roadmap tool. It needed an endpoint that answers straight away: send an insight, get a link back. Survey responses arrived in bulk, mostly noise, and each needed judging before filing. I tried having the scheduled survey job write to Productboard directly. It was harder to debug and recover than handing a batch file to a separate filing step I had already proven.

## Decision

I run two lanes. The direct lane is a webhook on Vercel, a hosting service: a web address that takes single insights from tools where a person has already confirmed the content, and files each one at once. A scheduled pipeline on GitHub Actions, GitHub's automation service, runs the channels an AI model judges. One step fetches and judges items and writes a batch; a second step files the batch and posts a summary to Slack. When the support-desk channel arrived, I put it on the scheduled pattern too.

## Alternatives rejected

- Scheduled jobs writing straight to Productboard: no batch to inspect or replay when filing failed.
- Every channel calling the direct lane: it would have needed batch handling, judging and summaries, which belong to bulk sources.

## Consequences

- Two places to deploy and two sets of credentials to keep alive, each needing its own health check.
- The shared rules for customer matching, tags and failure handling must stay identical across both; see [0002](0002-one-engine-many-channels.md).

## Date

2026-06, confirmed for new channels 2026-07.
