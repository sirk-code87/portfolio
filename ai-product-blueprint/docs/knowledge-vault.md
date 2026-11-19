# Knowledge vault

Obsidian is a note-taking app that works on a folder of markdown files on disk. I use one vault as the team's memory and its task system. Any agent can read and write it with ordinary file tools, people browse it in the app, and git keeps its history.

The properties at the top of each note (a short block of named fields) are what make it work for agents. They turn notes into records that views can filter and checks can validate.

## Why a vault

| Need | How the vault meets it |
|---|---|
| Agents of any brand can use it | Plain markdown and properties; no database or vendor format |
| Reasoning survives after the meeting | Decisions, risks, questions and lessons are separate notes, linked to projects and to each other |
| Live views without a reporting tool | Obsidian Bases (saved table and board views over note properties) list open risks per project, tasks by status, signals by opportunity |
| History and review | The vault is a git repository with automatic backups, so every agent change can be read and reverted |
| Tasks next to the work | Each task is a note, linked to the project and decisions it relates to |

## Folder model

| Folder | Holds |
|---|---|
| Index | Maps of content: one hub note per area that lists and embeds views of everything in it |
| Projects | One folder per project with a hub note; the project's records appear in it through embedded views |
| Product | Tactical opportunity trees, each rooted on a key result: opportunities, signals and first slices, plus PRD drafts, personas and story maps |
| Timestamps | Daily notes, meeting notes, session notes, and the records: decisions, risks, questions, lessons |
| Resources | The playbook and frameworks agents load: operating model, evidence rules, PRD format, source registry |
| System | Note templates, the agreed property values, the event log, checks and routines |
| Tasks | One note per task, plus board and list views |

## Note types

Every note has a `type` property, a creation date and links to its hub. Each type adds its own required properties.

| Type | Required properties | Created by |
|---|---|---|
| Signal | Source channel, who is affected, problem area, evidence type, why it matters | Meeting processing, a pipeline, or me |
| Opportunity | Linked signals, signal count, latest signal date, objectives, value checks passed, demand tier | Me, or an agent proposal I accept |
| Objective | Investment type, objective type (committed, aspirational or learning), trigger, demand tier and its sources, lagging metric | Me |
| PRD | Status, linked opportunity, owner; body follows the PRD format | Agent draft, edited by me |
| Decision | Statement, door type, decider, category, source, project, optional review date and superseding decision | Session log or meeting processing |
| Risk | Statement, likelihood and impact before and after treatment, owner, review date, status | Session log |
| Question | Statement, owner, whether it blocks, the answer when found | Session log |
| Lesson | The rule, its trigger, evidence, status (active, promoted, retired), hit count, where it was promoted to | Session log, after I confirm it |
| Session | What was done, decisions and records created, surprises | Session log at the end of each working session |
| Task | Status, area or project, due or scheduled date only when real, recurrence rule if recurring | Me or an agent, through the task tool |

Decisions are atomic: one decision per note. A decision note names who decided and who gave input separately, so a lens's draft never reads as the decision.

## Tasks

Tasks run on TaskNotes, an Obsidian plugin that stores each task as a note and provides boards, calendars and a command-line tool.

- **Statuses:** open, in progress, Delegate (handed to the agent team), Review (waiting for my decision), done. See the handoff lane in [agent-team.md](agent-team.md).
- **Agents change tasks through the task tool's command-line interface.** It keeps the properties valid. A scripted bulk edit of task properties once cut the bodies off a batch of task files, so direct rewrites of task properties are banned.
- **Recurring tasks stay open.** Each completion is recorded against its date and moves the next scheduled date on. Marking a recurring task done ends the series, so a check blocks that.
- **The bar for a new task is high.** An agent creates a task only when it is confident the work deserves one. A missed task can be added later; clutter costs attention every day.

## Templates and checks

Each note type has a template that fills the required properties. A check runs after every write by an agent: it validates the note's properties against the agreed values and reports anything missing or unknown.

The agreed values live in one standards note. Before I wrote it down, the scripts that create notes each used their own values, and views missed records as a result. I consolidated the vocabulary, cleaned the existing notes and made the check enforce it. After that, a full validation run across the vault came back with no warnings.

## How agents use the vault

| Action | Tool |
|---|---|
| Read and edit notes | Ordinary file tools |
| Search, backlinks, property queries | The Obsidian command-line tool, which uses the app's index; needs the app running |
| Rename or move a note | Through Obsidian, so links to it are updated |
| Create or change tasks | The task tool's command-line interface |
| Instructions for a folder | A short instruction file in that folder, loaded when the agent works there |

The root instruction file stays short: what the vault is, the rules that apply every turn, and where to look for the rest. Each area keeps its own instruction file, so an agent working on tasks reads the task rules and nothing else.
