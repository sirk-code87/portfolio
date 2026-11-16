# Architecture

The coach was one web application with three entry points, and conversations lived in the browser. It ran the conversation with the AI model and the confirm step. Filing belonged to the sibling [insight-relay](../../insight-relay) through its direct lane (a web address other tools send a single note to, which files it at once and sends back a link). The relay held the only Productboard credential (Productboard is the tool where the product team collects and prioritises feedback). The diagrams below describe the design at the level it can be shared, with generic names, hosts and identifiers.

Terms used below: an LLM (large language model) is the AI model that writes the coach's replies; the model gateway is one service that every model call passes through.

## 1. Context

```mermaid
flowchart LR
    subgraph surfaces["Where people use it"]
        web["Web app<br/>company sign-in"]
        slack["Slack<br/>message and slash command"]
        tool1["Account ops console<br/>pop-up window"]
        tool2["Voucher admin portal<br/>pop-up, awaiting review"]
    end
    subgraph coach["Insight coach"]
        chat["Chat step<br/>replies word by word"]
        extract["Draft step<br/>draft insight"]
        file["File step<br/>confirmed insight"]
    end
    gw["Model gateway"]
    llm["LLM"]
    relay["insight-relay<br/>direct lane"]
    pb[("Productboard")]

    web --> chat
    slack --> chat
    tool1 --> chat
    tool2 -.-> chat
    chat --> gw
    extract --> gw
    gw --> llm
    file -->|"per-caller key"| relay
    relay -->|"note, customer, creator, tags"| pb
    relay -.->|"company candidates"| extract
```

Each surface runs the same coach and the same confirm card. The web app signed colleagues in through company single sign-on (one company login for every internal tool). Slack used a direct message and a slash command (a typed `/` shortcut). The two embedded surfaces signed users in with a token signed by the host tool (diagram 4). Three routes, or server addresses, did the work: a chat route that sent each coaching turn back word by word as it was written, an extract route that turned the conversation into a draft insight for the card, and a file route that sent the confirmed insight to the relay. The model sat behind the gateway, so every caller and the evaluation suite shared one key and one usage view. The relay created the Productboard note and its links, and served the list of candidate companies the card offered. The voucher admin portal's integration was built and awaiting review at handover, shown dashed.

## 2. A coaching session, through confirm and file

```mermaid
sequenceDiagram
    autonumber
    participant B as Browser, holds the conversation
    participant C as Coach server
    participant M as LLM via gateway
    participant R as insight-relay
    participant PB as Productboard
    B->>C: chat turn with full history
    C->>C: check session, rate limit, role
    C->>M: coaching prompt and history
    M-->>B: question or playback, word by word
    Note over B,C: up to three questions, or file as-is at any point
    B->>C: extract with full history
    C->>M: structured extraction request
    M-->>C: draft insight
    C->>C: validate, tidy values, cap tier, check verbatim text
    C->>R: request company candidates
    R-->>C: best matches
    C-->>B: card with every field
    B->>B: raiser corrects and confirms
    B->>C: file confirmed insight
    C->>C: set raiser and surface from the session
    C->>R: insight with per-caller key
    R->>PB: create note, link customer and creator, add tags
    PB-->>R: note link
    R-->>C: filed
    C-->>B: completion screen
```

The browser sends the whole conversation with every request, so any server instance can answer, and a page refresh loses an unsent draft by design. Chat and extraction are separate calls. Chat runs at a higher temperature (the setting that gives the model more variety) and replies word by word; extraction runs at a lower temperature and returns structured fields. The server checks every draft before it reaches the card. It validates the shape, maps loose values to the nearest allowed one, caps the evidence tier and keeps the raiser's text verbatim. At filing time the server writes identity and surface itself, overwriting anything the browser sent.

## 3. Conversation states

```mermaid
stateDiagram-v2
    [*] --> Opened
    Opened --> Coaching : first message
    Coaching --> Coaching : coach asks, up to three times
    Coaching --> Drafting : enough to draft
    Coaching --> Drafting : raiser chooses file as-is
    Coaching --> Coaching : message too terse, ask for more
    Drafting --> Reviewing : draft passes checks
    Drafting --> Drafting : extraction fails, visible retry
    Reviewing --> Coaching : raiser adds detail
    Reviewing --> Filing : raiser confirms
    Filing --> Filed : relay accepts
    Filing --> Reviewing : filing error, draft kept
    Filed --> [*]
    Opened --> [*] : abandoned
    Coaching --> [*] : abandoned
    Reviewing --> [*] : abandoned
```

The one way into Filed runs through Reviewing and an explicit confirm. File as-is skips the remaining questions and always shows the card. A draft that fails checks returns to Drafting with a retry the raiser can see, and a filing error returns to Reviewing with the draft intact. The visible retry is the corrected design; the README's lessons section covers the first version. An abandoned chat leaves nothing behind, because the server stores nothing.

## 4. Embedding in another tool

```mermaid
sequenceDiagram
    autonumber
    participant U as Signed-in user of the host tool
    participant H as Host tool server
    participant F as Coach in a frame
    participant C as Coach server
    participant R as insight-relay
    U->>H: click Give feedback
    H->>H: sign short-lived token with user, role, account and page
    H-->>F: open pop-up with the token
    F->>C: present token
    C->>C: verify signature, expiry and allowed host
    C-->>F: session credential, held in page memory only
    Note over F,C: framing limited to registered hosts
    F->>C: chat turns, then extract
    C->>C: strip emails and phone numbers from external users' text
    F->>C: file confirmed insight
    C->>R: insight with this host's own key
```

The host tool shows the coach in a frame, a page embedded inside its own. Company sign-in pages refuse to load inside a frame, and the host and coach sat on different sites, so they could share no browser cookies. The host tool's server therefore signs a token lasting a few minutes, bound to the user and the page they were on. The coach checks it and swaps it for a session credential lasting a couple of hours, kept only in page memory. A registry of hosts sets which sites may frame the coach and which relay key each one files with, so one host can be revoked and the others keep working. For users outside the company, the coach removed email addresses and phone numbers by pattern before the text reached the model or Productboard, kept their name and email out of every prompt, and turned attachments off where host screens could show guest data. [Decision 0006](decisions/0006-embed-with-signed-token.md) records the limits of pattern stripping.
