# Architecture

Four views of the agent OS as it stood in late 2026, plus the OpenCode design it replaced. Names are generic; the original files were private and were deleted at handover.

A few terms used throughout. Claude Code is Anthropic's AI assistant, which works with files and runs tools; OpenCode is an open-source assistant of the same kind that the system ran on first. A skill is a written procedure the assistant loads when needed. A subagent is a helper agent that runs a task in its own context and returns a result. A hook is a script that runs automatically before or after the assistant acts. MCP is the standard way the assistant connects to other software.

## 1. Layers

```mermaid
flowchart TB
    me(["Me, in a Claude Code session"])
    cmd["Commands<br/>session log, handoff run, lesson promotion"]
    persona["Persona skills<br/>9 interactive lenses"]
    skills["Skills<br/>domain packs, rituals, integrations"]
    deleg["Delegate agents<br/>lean, draft-only"]
    agents["Pipeline agents<br/>narrow, structured output"]
    hooks{{"Hooks<br/>send gate, style check,<br/>spec standard gate, note properties, event logging"}}
    vault[("Obsidian vault<br/>tasks, decisions, lessons, event log")]
    mcp["Tool connections (MCP)<br/>Slack, email, Productboard,<br/>CRM, Stripe, warehouse"]
    sched["Scheduled routines<br/>cloud routines, local jobs"]
    gate{{"Human gate<br/>send, merge, publish"}}

    me --> cmd
    me --> persona
    cmd --> skills
    persona --> skills
    persona -->|"dispatch 1-3"| deleg
    skills --> agents
    sched --> cmd
    deleg -->|"text only"| persona
    agents --> vault
    skills --> mcp
    hooks -.->|"intercept writes"| vault
    hooks -.->|"intercept writes"| mcp
    mcp --> gate
```

Commands and persona skills run in the main session, the conversation I am in. They dispatch delegates and pipeline agents, which return text or structured output and stop there, because a subagent cannot start another subagent. Skills hold the procedures and are the only layer that talks to outside systems through MCP. Hooks sit across every write: anything bound for Slack, email, a calendar or Productboard (the product roadmap tool) waits until I confirm the exact text, and outbound prose is style-checked first. Scheduled routines enter through the same commands I use by hand, so an unattended run follows the same rules as an attended one.

## 2. Weekly report built from an event log

```mermaid
sequenceDiagram
    participant E as Emitters<br/>session log, decisions, tasks, git, Productboard
    participant L as Event log, add-only
    participant A as Assembler
    participant G as Significance gate
    participant V as Checker, starts fresh
    participant W as Watermark and history
    participant C as Me

    E->>L: small factual event as work happens
    Note over E,L: each verb caps the wording, event format fixed at v1
    A->>W: read last watermark
    A->>L: read events after watermark
    A->>G: candidate events grouped by focus area
    G-->>A: keep state changes that pass the business test
    Note over A,G: leans towards leaving out, only dictated updates headline
    A->>V: draft report and source events
    V-->>A: cut claims and figures with no logged event
    A->>C: draft for review
    C->>W: approve, advance watermark
```

The first version summarised daily notes and then summarised the summaries, and an audit found invented figures and inflated verbs. In this design each emitter (a small script attached to a source such as the session log or git) writes one short event at the moment work happens, and events are only ever added. The watermark marks the last point already reported. The assembler reads only the events after it, so past weeks cannot resurface. The significance gate asks two questions: did a real state change happen, and does it matter to the business. It leans towards leaving things out. Each event's verb caps how strongly the report may describe it, and a checker with no access to the drafting context strips anything that has no event behind it. After the first live run I added a source gate: mechanical events made up most of the draft, so only updates I had dictated may headline.

## 3. Agent handoff lane

```mermaid
stateDiagram-v2
    [*] --> Open
    Open --> Delegate: I drag the card
    Delegate --> Delegate: queued, cap 3 tasks per run
    Delegate --> Done: draft written, nothing to decide
    Delegate --> Done: outbound draft placed in Slack or email
    Delegate --> Review: needs my decision
    Review --> Done: I decide
    Done --> [*]
```

The lane is two extra task statuses on the existing board. A runner fires three times each weekday, takes up to three Delegate tasks, routes each to one to three delegates through the `/team` dispatch table, and writes the primary delegate's combined draft onto the task. Until October every finished task went to Review, which held finished work, messages to send and real decisions together; 57 tasks built up there. After that change only decisions reach Review, and messages become drafts in Slack or email for me to send. Delegates never send anything themselves. Message tasks leaving the board after the draft is placed is my reading of the change; the exact status they took isn't recorded here.

## 4. OpenCode era against Claude Code era

```mermaid
flowchart LR
    subgraph oc["OpenCode, March 2026"]
        u1(["User"]) --> c1["19 commands"]
        c1 --> o1["4 phase orchestrators<br/>capture, discovery, delivery, rollout"]
        o1 --> s1["14 specialists"]
        o1 --> k1["32 skills"]
        sh1["9 shared context files"] -.->|"injected by reference"| c1
        k1 --> x1["External tools"]
    end
    subgraph cc["Claude Code, late 2026"]
        u2(["Me"]) --> p2["9 persona skills<br/>and 36 commands"]
        md2["CLAUDE.md"] -.->|"inherited"| p2
        p2 --> r2["/team dispatch table"]
        r2 --> d2["Delegates, draft-only"]
        p2 --> k2["102 skills<br/>with references"]
        k2 --> x2["Tool connections (MCP)"]
        h2{{"15 hook registrations"}} -.-> x2
        l2[("Event log, records,<br/>lesson digests")] -.-> p2
    end
```

The OpenCode design was a strict hierarchy. Commands went through the phase orchestrators (coordinating agents, one per phase of product work), and skills only ever called downward. It mapped the product operating model closely, and I had to add a consult mode so a command could get one specialist's view without running a whole phase. Moving to Claude Code removed the orchestrator layer. Personas became interactive skills that dispatch lean delegates directly. Shared context moved into one inherited `CLAUDE.md` (the instruction file the assistant reads at the start of every session) and skill references, and rules moved from prose into hooks. Memory moved from injected context files to records the system writes itself: the event log, decision and risk notes, and lesson digests.
