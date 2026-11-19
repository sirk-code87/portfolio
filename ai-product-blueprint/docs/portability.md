# Portability

What the design needs from an AI agent runtime, and what to do when a runtime lacks a feature. A runtime is the agent tool itself, such as Claude Code or OpenCode, both of which this design has run on.

## The contract

The design keeps its substance in files and uses runtime features only to apply it.

| Part | Held as | Portable because |
|---|---|---|
| Playbook, PRD format, evidence rules | Markdown in the vault | Any agent reads files |
| Note types and agreed values | Properties on notes, one standards note | Plain text fields |
| Routing rules and lens definitions | Markdown | Plain text; each runtime's subagent file wraps the same text |
| Tool connections | MCP servers | MCP (Model Context Protocol) is an open standard most agents support |
| Checks | Small scripts that take a file or an action and pass or fail it | The script is the same; only what calls it changes |

## What each part needs from the runtime

| Need | Used for | Claude Code | OpenCode | If the runtime lacks it |
|---|---|---|---|---|
| An instruction file read at the start of every session | Context, every-turn rules, where to look | `CLAUDE.md`, plus one per folder | `AGENTS.md` | Keep one canonical file; other names point to it in one line |
| Skills loaded on demand | Playbook procedures, rituals, the interactive lenses | Skill folders, each with a `SKILL.md` | Commands and agent prompts | Put the procedures in the vault and list them in the instruction file with when to read each |
| Subagents | Delegate lenses, pipeline steps | Agent definition files | Agent definition files | Run each lens in turn in the main session from the same lens text. Slower, and the lenses can see each other's work, so blind review is lost |
| Hooks before and after actions | Approval gate, PRD format check, note validation | Hook scripts on set events | Plugins | Run the same scripts as a git pre-commit check and a scheduled validation, and keep write access to systems of record off for the agent |
| MCP | Reading and drafting in other tools | Yes | Yes | Command-line tools or small scripts per tool |
| Scheduled runs | Daily routines, the handoff runner, the weekly report | Cloud routines | The machine's own scheduler | Any scheduler that starts the agent with a fixed command |

Hooks are the least portable part, and they carry the most important rules. So the approval gate also holds without them: an agent with no write key for a system of record cannot write to it, whatever its instructions say.

## What moving runtime taught me

I moved the system from OpenCode to Claude Code after about three months. About 110 definitions came across.

- **The playbook and note formats moved across as they were.** They were plain files.
- **A custom shared-context folder had to go.** In OpenCode I had built a convention for injecting shared context files into commands. The new runtime did not understand it, so I folded those files into the instruction file and into each skill's own reference files. Using the conventions a runtime reads natively costs less than maintaining one of my own.
- **Files pointed at an instruction file that no longer existed.** Dozens of files cited the old runtime's instruction file by name after the move. A search for references to the old names belongs in every migration.
- **Coordinating agents gave way to lenses.** The new runtime made it easy for skills to call subagents directly, so the phase coordinators became unnecessary. The playbook kept the phases.
- **Rules moved from prose into hooks.** Rules written as instructions were followed most of the time. Rules written as checks held until someone changed the check.

## Adopting it with another agent

1. Copy the playbook, note templates and standards note into the vault.
2. Write the instruction file: what the vault is, the every-turn rules, where to look.
3. Connect the systems of record over MCP with read access first; add draft and write access one tool at a time, each behind a check.
4. Add the lenses as subagents if the runtime has them, or as procedures if it does not.
5. Turn on the checks, through hooks if available, otherwise through git and a scheduled run.
6. Start the trust ledger on the first dispatch.
