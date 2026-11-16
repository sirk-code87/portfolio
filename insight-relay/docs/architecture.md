# Architecture

The original relay was two systems that shared rules and a destination. A service on Vercel, a hosting platform, ran the direct lane and the daily company sync from the CRM (the customer relationship system). A GitHub repository with scheduled jobs on GitHub Actions, GitHub's automation service, ran the AI-judged lanes. Both wrote to Productboard, the roadmap tool. I describe them here as one relay because they were designed, run and handed over as one.

Two terms recur. The **direct lane** is a webhook: a web address other tools send a single note to, which files it at once and returns a link. The **judged lanes** run on a schedule, fetch feedback in bulk, and ask an AI model (a large language model, or LLM) to judge each item against a written charter, the lane's rules for what counts.

## 1. Whole system

```mermaid
flowchart LR
    subgraph confirmed["Person confirms each note"]
        cweb["Coach on the web"]
        cslack["Coach in Slack"]
        cembed["Coach built into<br/>two internal tools"]
        skill["Claude Code skill"]
    end
    subgraph judged["AI judges each note"]
        survey["Survey tool"]
        calls["Call transcripts"]
        email["Email"]
        desk["Support desk"]
        inapp["In-app feedback"]
    end
    crm["CRM"]
    hook["Direct lane<br/>on Vercel"]
    sched["Judged lanes<br/>on a GitHub Actions schedule"]
    sync["Company sync<br/>daily"]
    attr["Shared customer matching<br/>company index and cache"]
    reg["Source tag list"]
    pb[("Productboard")]
    slack["Slack<br/>summaries, feed, alerts"]

    cweb --> hook
    cslack --> hook
    cembed --> hook
    skill --> hook
    survey --> sched
    calls --> sched
    email --> sched
    desk --> sched
    inapp --> sched
    reg -.-> sched
    hook --> attr
    sched --> attr
    attr -->|"notes linked to customers"| pb
    crm --> sync -->|"update companies"| pb
    pb -.->|"company list"| attr
    hook -.-> slack
    sched --> slack
```

About ten channels feed the relay. On the left, a person has already confirmed each note before it arrives: the insight coach (a chat assistant that helps staff write up feedback) on its web app, in Slack and built into two internal tools, plus a Claude Code skill (a packaged command for Anthropic's coding assistant) that product staff used to file notes without a roadmap-tool login. In the middle, nobody confirms individual notes; each scheduled lane fetches from its source and the AI judge decides what to file. Both lanes match the customer company through the same code, which reads the company list the CRM sync keeps current. Slack receives summaries from the judged lanes, an optional feed of confirmed notes for each tool, and failure alerts. The in-app feedback lane was built and running in dry-run, a test mode that files nothing, when I handed the relay over.

## 2. A judged lane run

```mermaid
sequenceDiagram
    autonumber
    participant C as Scheduler
    participant L as Lane job
    participant Src as Source system
    participant Led as List of filed items
    participant J as AI judge on the Claude API
    participant PB as Productboard
    participant S as Slack
    C->>L: start on the lane's daily or weekly slot
    L->>Src: fetch items since the last run
    Src-->>L: new items
    L->>Led: drop items already filed
    L->>J: judge each item against the lane charter
    J-->>L: pass or fail with a reason
    L->>L: remove personal details and add source tag
    alt dry run, the default
        L->>S: graded summary marked DRY, nothing filed
    else live, switched on explicitly
        loop each passing item
            L->>PB: match company and create note
            PB-->>L: note link, or a refused tag
            L->>PB: if refused, remove the tag and retry
        end
        L->>Led: record filed items only
        L->>S: summary with links to the new notes
    end
```

Each lane runs on its own schedule: the busier sources daily, the support desk and in-app feedback weekly. The list of filed items records an item only after it has been filed, so a failed item is retried on the next run. Each lane has its own written charter. The call lane, for example, files product gaps only, ignores pricing and competitor talk, caps itself at a few insights per call, and keeps a short quote as evidence with names replaced. The survey lane holds an answer that proposes a fix until a second customer raises the same topic. Every lane starts in dry-run and files only after an explicit switch, so a misconfigured lane posts a summary and changes nothing.

## 3. The direct lane

```mermaid
sequenceDiagram
    autonumber
    participant Co as Caller such as the coach
    participant R as Direct lane
    participant K as Shared company cache
    participant PB as Productboard
    participant S as Slack
    Co->>R: send insight with the caller's own key
    R->>R: check the key and the note's fields
    alt bad key or invalid note
        R-->>Co: rejected, nothing filed
    end
    R->>K: get company list
    alt cache hit
        K-->>R: list, refreshed in the background if stale
    else miss or cache error
        R->>PB: fetch company list directly
        PB-->>R: company list
    end
    R->>R: match the customer company
    R->>PB: find or create the raiser as a user
    R->>PB: create note with customer, creator, owner and tags
    alt a tag is refused
        R->>PB: remove the tag and retry
        R->>S: alert that a tag was dropped
    end
    PB-->>R: note link
    R-->>Co: success with the note link
    R-)S: after responding, post summary and link to the tool's Slack feed
```

Each caller holds its own key, so one caller can be revoked without touching the others. The relay compares keys in constant time, a check that reveals nothing through how long it takes. It also checks that each note has the agreed fields, and rejects a bad key or a malformed note before filing anything. The company list sits in a shared cache on Vercel with a short freshness window. When the cached copy is out of date, the relay serves it and refreshes it in the background (a pattern called stale-while-revalidate). If the cache fails, the relay fetches the list directly and carries on. The raiser is created as a Productboard user the first time they appear, with a retry because a newly created user is not always visible straight away. The Slack feed posts a summary and a link, never the raw conversation, so personal details typed into the coach stay out of Slack. The feed is off for a tool until its channel is set up.

## 4. Matching a note to its customer

```mermaid
flowchart TD
    start(["Insight arrives"]) --> conf{"Company confirmed<br/>on the coach card?"}
    conf -->|"yes"| multi{"Several customers<br/>named?"}
    multi -->|"yes"| split["One note per customer"]
    multi -->|"no"| cust["Customer is that company"]
    split --> cust
    conf -->|"no"| idx{"A distinctive word from<br/>web domain or name matches<br/>exactly one company?"}
    idx -->|"yes"| cust
    idx -->|"no match or several"| unm["File with no company<br/>and flag for the area PM"]
    cust --> raiser["Raiser linked as a<br/>Productboard user and creator"]
    unm --> raiser
    raiser --> owner["Owner is the product PM"]
    owner --> file(["File the note"])
```

Matching answers two questions: which customer does this belong to, and who raised it. When a colleague files through the coach, the coach shows its best company matches on the confirm card and the colleague picks; they know the answer at filing time, so I asked them. Otherwise the relay builds an index of words that appear in exactly one company's name or web domain, and accepts only a single clear match. I ruled out loose name matching because it attached feedback to the wrong customer, which is worse than no attachment. A note with no match is filed without a company and flagged for the product manager (PM) for that area. It is never filed under the company's own name and never guessed to be a prospect. Filing never waits on matching.
