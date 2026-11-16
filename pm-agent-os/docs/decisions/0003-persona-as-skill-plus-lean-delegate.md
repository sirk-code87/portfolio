# 0003: Each persona is an interactive skill plus a lean delegate

## Context

After the move to Claude Code, each persona (a role lens such as product or legal) existed twice over: a slash command and a persona agent carrying the same instructions, plus per-domain copies. When the business context changed, about 40 configuration files went stale at once. A persona also had two jobs that pull apart. As a counterpart it needs back-and-forth dialogue with me. As a worker it needs to take a task and return a result. Claude Code subagents (helper agents that run a task in their own context) return a result and cannot start further subagents, so a persona agent could not route work.

## Decision

I split every persona into two forms.

- An **interactive persona skill** runs in the main session, the conversation I am in. It loads a domain lens from a shared routing file, announces it, loads that persona's lesson digest (a short list of past corrections), applies a challenge charter, and dispatches workers with the domain rubric appended.
- A **lean delegate agent** holds the lens, thinking defaults and vault context, and carries no routing and no session content. Persona skills, the handoff runner and any other workflow can dispatch it.

Domain knowledge lives in packs (payments, unit economics, presales) that any persona can load, kept in one place.

## Alternatives rejected

- **One agent per persona that also routes.** Subagents cannot dispatch subagents, and the counterpart role needs dialogue.
- **Per-domain commands or per-domain persona copies.** Every copy is another file to go stale.
- **A `/team` orchestrator agent.** I corrected this in September: `/team` is the roster and dispatch table, and the main session does the routing.

## Consequences

- All nine personas became dispatchable in September 2026 once six delegates were built.
- The product delegate became the model the others were written from.
- Delegates did useful independent work: in one session the data delegate found an analytics event that answered a question the product delegate had called unanswerable.
- They also missed things a person caught: a design delegate's copy review missed duplicated controls that I spotted from screenshots.

## Date

July 2026 (split), September 2026 (all nine dispatchable)
