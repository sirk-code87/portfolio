# 0006. Other tools embed the coach through a signed, short-lived token

## Context

Two internal tools needed a feedback route, and one was preparing to open to customer users. The first attempt at a native feedback form in one of them collected requests as they came, unqualified, which was the problem the coach existed to fix.

## Decision

- The host tool opens the coach in a pop-up window. Feedback from the host goes through the same coach, card and relay as every other surface, tagged with the host's surface name.
- The host's server signs a token (a tamper-proof pass) lasting a few minutes, bound to the user, their role, the customer account and the page they were on.
- The coach checks the token and swaps it for a session credential lasting a couple of hours, held only in page memory.
- A registry of hosts defines which sites may show the coach in a frame and which relay key each one files with. Each host has its own key, compared in constant time (a check that takes the same time whatever the input, so timing gives nothing away), so one host can be revoked alone.
- For users outside the company: email addresses and phone numbers are stripped from their text before the model sees it and before filing, their name and email stay out of every prompt, and attachments stay off where host screens could show guest data.

## Alternatives considered

- A native form in each tool: collects requests without coaching, and each team would rebuild the same intake.
- Company single sign-on inside the frame: sign-in pages refuse to load in frames, and the two sites could not share cookies.
- Opening the coach in a new tab: kept as a fallback, since it loses the page context the token carries.
- A separate intake creating tickets for the host tool's team: skips the product team's shared triage.

## Consequences

- The account ops console's embed went live. The voucher admin portal's integration, which included customer users, was built and awaiting review at handover.
- The first live host received almost no human feedback in its early days. Building the route was faster than earning its use.
- Pattern stripping is a mitigation. It misses names and free-form addresses.
- The external variant needed its own copy of the prompts, and I dropped judged evaluations as its launch condition. [Evaluation](../evaluation.md) covers that decision.

## Date

2026-10
