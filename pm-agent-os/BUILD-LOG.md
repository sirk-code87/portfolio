# Build log

Reconstructed after the event, to the month. The original system and its code are private.

Newest last. Each era opens with what changed and why; the entries under it are the milestones and reversals of that period.

## Era 0: Seeds (October 2025 to January 2026)

I started from a generic library of business and product subagents (helper AI agents, each with one role) and early use of Claude Code (Anthropic's AI assistant, which works with files and runs tools) on one integration.

### 2025-10  Imported a generic library of business and product agents
About 27 role agents (product manager, business analyst, UX researcher, legal adviser, scrum master and others). The specialist roster of the next era reused these names.

### 2026-01  Made the planning agents slice work by user value
The first rule applied to every agent: release by user value, never split work by technical layer.

## Era 1: OpenCode orchestrators and specialists (February to mid-May 2026)

I tried to make the product operating model runnable on OpenCode (an open-source AI coding assistant). Commands called phase orchestrators (coordinating agents for capture, discovery, delivery and rollout), which called specialists and skills (written procedures the assistant loads when needed). Only skills touched outside tools.

### 2026-02  Built commands for end of day, meetings, PRDs and tasks
PRDs are product requirements documents. The vault became somewhere work runs as well as a place to keep notes.

### 2026-02  Demonstrated a live AI documentation workflow to the product team
Generated go-to-market plans and user stories from a codebase in a team meeting; team set-up followed.

### 2026-03  Added a meeting pre-read skill and template
The first reusable meeting-prep skill.

### 2026-03  Connected Productboard and audited the agents' instructions
The connection used MCP, the standard way AI assistants connect to other software. It started the Productboard (product roadmap tool) integration that later fed every insight pipeline.

### 2026-03  Mapped the system architecture, with hierarchy rules and a consult mode
87 components: 19 commands, 4 orchestrators, 14 specialists, 32 skills, 9 shared context files. Consult mode let a command ask one specialist for a view without running a whole phase.

### 2026-03  Removed hierarchy breaches and overlapping roles
A skill that called specialists was rewritten, direct command-to-specialist shortcuts were removed, two assistant roles folded into the chief-of-staff command, and a requirements skill split into PRD and user-story writers.

### 2026-03  Added leadership-report and product briefing skills; packaged the set for colleagues
The chief-of-staff routing table grew, and I packaged the set for colleagues for the first time.

### 2026-04  Wired qualification skills into the capture agents
The first of 67 logged tooling-investment sessions between April and October.

### 2026-04  Cut the product operating model document from 2,042 to 515 lines
Every agent that read the operating model got a smaller context.

### 2026-04  Cut start of day from 68-90 tool calls to about 40-55
Cheap enough to run every morning.

## Era 2: Claude Code migration and hooks (May to June 2026)

I moved every agent and command to Claude Code and started enforcing rules in hooks (scripts that run automatically before or after the assistant acts). Why I left OpenCode isn't recorded here.

### 2026-05  Migrated 110 agent and command definitions from OpenCode to Claude Code
`CLAUDE.md`, the instruction file the assistant reads at the start of every session, became the single configuration inherited by every subagent.

### 2026-05  Added six lifecycle hooks
Write guards for governance files, checks on the properties at the top of each note, and meeting and task nudges. Rules moved from prose into code.

### 2026-05  Turned starred emails into tasks; added a Friday tooling-budget prompt
Starred emails became reminder tasks; a weekly prompt asked which instinct to promote into a skill.

### 2026-06  Published an insight-filing skill to the team's shared skills repository
Any product manager could file Productboard notes without holding credentials.

### 2026-06  Built a Productboard MCP server with 30 tools
The first team-facing infrastructure.

### 2026-06  Relayed survey responses into Productboard automatically
The first automated feedback lane: a webhook relay, an endpoint the survey tool calls with each response.

### 2026-06  Designed a convener for blind parallel critique by chosen lenses
Classify the decision, propose a panel, let me curate it, critique in parallel, synthesise with conflicts shown. The precursor of `/team`; I have no record that it shipped.

### 2026-06  Added risk-based change gating to the ticket-triage engine, with 60 tests
Reframed from a router choosing the cheapest model to a gate deciding which changes a product manager may make.

## Era 3: Native consolidation and ledger-based pipelines (July 2026)

I removed everything Claude Code did not support natively and rebuilt the pipelines on logs and ledgers (running records of facts), after the weekly report showed what summarising summaries does.

### 2026-07  Launched `/pm` with domain packs, a lens announcement and a challenge charter
One front door; the payments lens arrives with its rubric, ready to use.

### 2026-07  Cleared out OpenCode conventions and folded the shared-context folder into `CLAUDE.md`
71 files cited a configuration file that no longer existed; 18 shared files (2,521 lines) became 45 extra lines of `CLAUDE.md` plus skill references. Persona agents slimmed to delegates. I rejected per-domain persona copies because copies go stale.

### 2026-07  Launched the customer-call insight pipeline
Idea to production in two days, judged on evidence (96% precision on 27 hand-checked gaps) in place of a two-week calendar trial.

### 2026-07  Sent digests to my direct messages only
I stopped digests falling back to a shared team channel.

### 2026-07  Built the CRM to Productboard sync on Vercel
The CRM is the customer database; Vercel is a web hosting service. The sync replaced an unused workflow-automation flow and a cloud routine. It keeps nothing between runs and only updates existing records. It alerts on failures only and sends a weekly heartbeat, so silence means it is down.

### 2026-07  Moved services from personal accounts to company accounts
I had started two services on personal hosting and repository accounts. Moving them gave the team continuity. I also replaced an access token that had reached the code history.

### 2026-07  Wired one Knowledge Base into the skills
One home for strategy and customer personas, with volatile figures kept in a single file.

### 2026-07  Ran tasks unattended; moved domain packs to shared routing
Churn, weekly report and retrospective runs went ahead without me; every persona reads the same routing file.

### 2026-07  Set up an overnight issue runner on scheduled cloud routines
Issues a person labels as ready become pull requests (proposed code changes for review) by morning. Labels move each issue through fixed stages. Fixed per-run counts gave way to batch sizes that adjust to the queue.

### 2026-07  Rebuilt the weekly report on an event log and froze the event format at version 1
Capture tests 32/32 and report tests 16/16. Five scanning subagents and the daily-note and email scans retired.

### 2026-07  Ran the new weekly report live for the first time; added a source gate
About 68% of the first draft came from mechanical events while dictated updates were 8% of the log, so only dictated status updates may now headline.

### 2026-07  Specified the prototype host: 17 issues, ten ready for agents
Specification passes by the product and engineering lenses fed the overnight runner directly.

### 2026-07  Gave each session its own note; consolidated the note properties
Emitters (the scripts that log events) had drifted from the metadata standard; afterwards 3,050 files validated with zero warnings.

### 2026-07  Added chat and email as event sources
A backward-compatible addition to the list of allowed source values, so no version bump.

## Era 4: Personas and atomic records (August 2026)

I added the lenses nobody was raising (legal, finance, data, commercial) and gave each risk, question and decision its own note, searchable per project.

### 2026-08  Replaced project risk registers with one note per risk
Risks became searchable per project.

### 2026-08  Expanded to nine personas with a `/team` roster
Commercial, data, design, legal, marketing and finance joined product, engineering and operations; the product-marketing command folded into marketing.

### 2026-08  Closed the prototype host as won't-do and used Vercel
Vercel already covered the hosting need. This reversed the spec's own rejection of preview deployments (temporary test sites built for each change).

### 2026-08  Built a self-installing Claude Code pack for the product team
Four persona skills and 18 agents, with a runbook (step-by-step set-up guide) checked by installing into a clean environment.

### 2026-08  Added question records with owners and a definition of done
Wired into the product, operations, engineering and legal personas.

### 2026-08  Deleted the OpenCode runtime; replaced decision logs with one note per decision
One configuration tree. Every decision now carries a named decider and a door type: one-way if it is hard to undo, two-way if it is easy.

### 2026-08  Adopted the change pipeline for code changes sized for a product manager
Ruled out and then adopted in the same month once I saw it could carry the risk gate and its receipts: classifier, sandboxed implementation, draft pull request, human merge. The bespoke triage engine stayed as reference.

## Era 5: Handoff lane and dispatch layer (September 2026)

Delegable tasks were sitting on my board. I gave them a lane that rides on task status.

### 2026-09  Kept `/team` as a roster, with routing in the main session
A correction I made before the dispatch layer was built. The main session is the conversation I am in.

### 2026-09  Launched the agent handoff lane on task status
Drag a card to Delegate; a scheduled runner routes it to one to three delegates and writes the draft onto the task. I dropped a heavier first design (a tag, a block of note properties, a ledger, a folder and a separate review task) for two statuses.

### 2026-09  Added the `/team route` dispatch table and six new delegates
All nine personas became dispatchable, and the same table served the runner and any other workflow.

### 2026-09  Built a customer meeting digest for account management
The first tooling I built for another team, run in shadow mode alongside the old process before cut-over.

### 2026-09  Logged two-way decisions at the moment I made them
Decision capture moved from end of day to the point of decision.

### 2026-09  Banned scripted rewrites of note properties
I decided bulk edits to task metadata go through the task tool only, after a scripted rewrite damaged task files.

### 2026-09  Added style checks on outbound text; prompt tests passed 137 of 142
House voice checked before anything leaves; a 12-scenario test set (eval) set the baseline.

## Era 6: Lesson memory and hardening (October 2026)

The same corrections kept recurring and the review column had become unreadable.

### 2026-10  Drafted weekly Productboard release notes for the product channel
A recurring team communication drafted end to end.

### 2026-10  Built lesson memory: notes, per-persona digests, capture, loading and promotion
Corrections become notes with a trigger and a hit count; a script builds a capped digest per persona. I rejected Claude Code's built-in agent memory because it had no evidence gate or audit trail.

### 2026-10  Revised the delegates, `/team`, the handoff runner and the question protocol
Several skills retired. The detail of the revision isn't recorded here.

### 2026-10  Found the first week's lessons reached no persona digest
Nearly every lesson was tagged general, because corrections happened in sessions with no persona in use.

### 2026-10  Planned and shipped payments ops portal integrations through `/team` agents
Two-way doors defaulted to proposals so agents could proceed; one-way doors held as stop points.

### 2026-10  Added a request-to-code routine for portal feedback to start of day
Reuses vault skills and `/team` agents, needs no new secrets, and stops at a pull request.

### 2026-10  Tested 50 strategy claims with data, finance, legal and engineering agents
An independent finance agent rebuilt the model from scratch.

### 2026-10  Had 22 agents draft about 135 open tasks
Grouped by lens, draft-only, with outputs written into the task notes.

### 2026-10  Narrowed the review column to my decisions; outbound drafts go to Slack or email
57 tasks of three kinds had collected in one column, and drafts sat in notes I did not open.

### 2026-10  Limited the PRD gate to Productboard record writes
It had been blocking edits to standalone guides.

## Era 7: Handover (November 2026)

I left the company and handed over what the team needed.

### 2026-11  Listed every automation by what it feeds; deleted local configuration
Team-facing services stayed with alerts re-pointed, routines on my personal account were deleted, laptop-bound jobs were switched off. The handover notes list which repositories merge code changes automatically once tests pass (the ones I owned) and which wait for engineer review.

### 2026-11  Moved general tooling to a personal vault; set the team commands aside
Persona commands and work agents kept as an inventory for a future role.
