# Systems of record

A system of record is the tool the company treats as the source of truth for one kind of fact. This file sets which tool owns which fact, what agents may do with each, and how they connect.

MCP (Model Context Protocol) is the open standard most AI agents use to connect to other software. An MCP server wraps one tool and gives the agent a set of actions it can call, such as "search companies" or "create a draft".

## Who owns which fact

| Fact | Owner | Agents read | Agents write | Approval |
|---|---|---|---|---|
| Customers, accounts, contracts, health | CRM | Yes | No | n/a |
| Customer evidence: feedback, insights, requests | Roadmap tool | Yes | Add new evidence, linked to the customer's company | None for evidence from a trusted pipeline; every other write is approved |
| Objectives, opportunities, features, priorities | Roadmap tool | Yes | Draft in the vault, publish through the gate | Every publish |
| Work in progress, sprints, defects | Delivery tool | Yes | Draft tickets and comments only | Every write |
| What customers said | Call-transcript tool | Yes | No | n/a |
| Usage, revenue and product metrics | Data warehouse and web analytics | Yes, through saved queries | No | n/a |
| Errors and feature flags | Monitoring and flag tools | Yes | No | n/a |
| Payments | Payment provider | Yes, with a read-only key | No | n/a |
| Code | Code host | Yes | Branches and draft pull requests | An engineer reviews every merge |
| Messages | Chat and email | Yes | Drafts placed in the chat or mail tool | I send each one myself |
| Decisions, reasoning, risks, questions, lessons, the event log, tasks | Vault | Yes | Yes | Checked by note-format rules |

The vault owns what the other tools lose: why a decision was made, who made it, what was tried and rejected, and what the agents were corrected on.

## Rules

**Read from the owner every time.** An agent that needs a customer's status asks the CRM in that session. It does not keep a copy in the vault. A vault note stores the link or the record's ID and the reasoning around it.

**Each tree has one home.** The strategic opportunity tree (outcomes, objectives, key results, initiatives) is published where the company plans, usually the roadmap tool. Tactical trees are a team's working notes in the vault, each rooted on the key result it serves and linking to the published records and customer evidence it draws on. A node lives in one place and is linked from the other; copying it into both splits the evidence. See [product-playbook.md](product-playbook.md) for the two levels.

**Writes are narrow.** A sync between two systems only updates records that already exist and keeps nothing between runs, so a failed run can be repeated safely. Agents add evidence and draft changes. They do not delete, merge or re-parent records.

**Every write to a system of record waits for approval.** A hook (a script that runs before the agent acts) stops the write and shows me the exact content. A second check runs first for writes with a house standard: a PRD (product requirements document) cannot reach the roadmap tool until it passes the format check in [product-playbook.md](product-playbook.md). I scoped that check to roadmap records after it blocked edits to ordinary documents.

**Messages are drafts.** Agents place a draft in the chat or mail tool. I read it there and send it. Drafts left inside task notes went unread, so they moved to the tools where I already read messages.

## Connections

| Choice | Rule |
|---|---|
| Protocol | MCP by default. It works across agent runtimes, so a connection set up once serves every agent. |
| When MCP costs too much | Some MCP servers load a long list of actions into every session and use up the agent's working memory. For a tool the agent uses all day, a small command-line tool or script is cheaper. I replaced the task manager's MCP server with its command-line tool for this reason. |
| Accounts | Company accounts from the first day. Services I started on personal accounts had to be moved before handover. |
| Keys | One narrow key per job, read-only where reading is enough. Keys live in the environment, never in files the agent writes. |
| Third-party skills | Off by default if they can set up services or issue keys. They act without an approval step. |
| Scheduled routines | Run through the same commands and checks as a session I attend, so an unattended run follows the same rules. |

## Failure cases this design covers

| Failure | What stops it |
|---|---|
| An agent reports a customer status that changed last week | Reads go to the owner each time; the vault holds no copy |
| A summary invents a figure | Reports draw only on logged events, and a checker that starts fresh cuts claims with no event behind them (see [workflow.md](workflow.md)) |
| An agent publishes a half-written spec | Format check and approval gate on every roadmap write |
| A message goes out in the wrong tone or to the wrong channel | Drafts only; I send |
| A key leaks through the code history | Keys in the environment; one narrow key per job, so a leaked key can do little |
