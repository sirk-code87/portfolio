# 0002: Draft-only outbound, enforced by hooks

## Context

Agents in the OS could reach Slack, email, calendar and Productboard (the product roadmap tool) through MCP, the standard way Claude Code connects to other software. An instruction in `CLAUDE.md` (the instruction file the assistant reads at the start of every session) saying "never send without asking" depends on the model reading and obeying it in every session, including unattended runs. The earlier OpenCode setup held all its rules as prose, and prose alone could not make a rule hold.

## Decision

I moved the outbound rules from prose into Claude Code lifecycle hooks, scripts that run automatically before or after the assistant acts.

- A hook that runs before each tool call intercepts every send, draft creation and calendar write, shows the exact text and waits for my confirmation.
- An outbound style check runs on text before it leaves, so the house voice is checked in code.
- A gate on Productboard record writes requires the house PRD (product requirements document) standard first.
- Delegates and scheduled runners are draft-only by design: they return text, and the only outside writes they make are drafts in Slack or email that I send myself.
- Automated code work in code another team owns stops at a pull request, a proposed change for that team's engineers to review. Only repositories I owned merged changes automatically once tests passed.

## Alternatives rejected

- **Prose rules only.** Unenforceable, and invisible in unattended runs.
- **Removing outbound integrations from agents entirely.** Drafting messages in place was the point; a gate at the moment of effect keeps that.
- **Third-party skills that act without an approval step.** I switched off those that could set up services or issue keys.

## Consequences

- Fifteen hook registrations across four trigger points by handover.
- Hooks need maintenance: the PRD gate was too broad at first and blocked edits to standalone guides, so I limited it to Productboard record writes.
- The rule covers agent actions inside the vault. Code changes needed their own record: the overnight runner's merge tiers (automatic in my own repositories, engineer review elsewhere) lived in each repository's settings with no decision record, so the handover notes had to spell them out. A guardrail that matters should be written down and checked wherever it applies.

## Date

May 2026 (first hooks); outbound style check September 2026; drafts moved into Slack and email October 2026.
