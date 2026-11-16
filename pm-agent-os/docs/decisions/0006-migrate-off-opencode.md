# 0006: Migrate from OpenCode to Claude Code

## Context

From February to May 2026 the system ran on OpenCode, an open-source AI coding assistant: 19 commands, 4 phase orchestrators (coordinating agents, one per phase of product work), 14 specialists, 32 skills and 9 shared context files pulled in by reference. It was packaged for colleagues as well. By May it held 110 agent and command definitions.

## Decision

I moved all 110 definitions to Claude Code, Anthropic's AI assistant, in one step, with `CLAUDE.md` (the instruction file read at the start of every session) as the single configuration. I added lifecycle hooks, scripts that run automatically before or after the assistant acts, the following day.

My notes record the migration and its follow-up work. They do not record why I chose to leave OpenCode, so this record gives no reason.

## Alternatives rejected

None recorded.

## Consequences

- Hooks became available, and rules moved from prose into code (see [0002](0002-draft-only-outbound-enforced-by-hooks.md)).
- The first migration was a copy, so OpenCode conventions travelled with it. Six weeks later 71 files still pointed at a configuration file that no longer existed, and 18 shared context files (2,521 lines) sat in a folder Claude Code did not recognise. In July I cleared out the old conventions: the shared files became 45 extra lines of `CLAUDE.md` plus skill references, and commands with matching persona agents merged into skills.
- The OpenCode runtime stayed on disk until August, when I deleted it so there was one configuration tree.
- The lesson: migrate to the new tool's own constructs in the same pass, or schedule the clean-up at the time of the move.

## Date

May 2026; old conventions cleared July 2026; old runtime deleted August 2026
