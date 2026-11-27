# Build log

Reconstructed after the event, to the month. The original repositories and their commit history are private.

### 2026-06  Reuse the relay's filing contract for coached insights

The relay already filed Productboard notes for people without a Productboard login. The coach sends its confirmed insights through the relay's direct lane (a web address other tools send a single note to, which files it at once and sends back a link), in the same data shape. The date of the coach's first build isn't recorded here.

### 2026-07  Move the product choice out of the chat and onto the confirm card

The raiser picks the product on the card, and the coach's questions focus on the problem and evidence.

### 2026-07  Map evidence labels towards the weakest tier

An unrecognised tier label had rejected whole drafts. Synonyms now map to their tier and anything unknown becomes inference.

### 2026-07  Add a completion screen that says where an insight goes

Names the evidence pool, says individual replies are rare by design, and offers a way to book time with the product team.

### 2026-07  Launch to colleagues on the web with anonymous funnel events

Counts chats started, drafts reached, insights filed and failed filings as anonymous events. Conversation text stays in the browser.

### 2026-07  Add voice dictation to the message box

The browser's own speech input fills the box for the raiser to edit before sending.

### 2026-07  Add a Slack direct message and slash command front-end

Same coach and same confirm card, with the surface set by the server on every filed insight.

### 2026-07  Route all model calls through a model gateway

One key for the web app, Slack and the evaluation suite, and one place to watch usage.

### 2026-07  Add an evaluation harness with golden fixtures and a judge from another provider

Automated tests of the coach's answers: coaching and extraction test cases, majority of three, run one at a time, with a limited number of retries.

### 2026-07  Shorten titles at a word boundary

Long titles had been cut mid-word or rejected for exceeding the length limit.

### 2026-08  Split change requests into problems in the customer success mode

Each submission files a note and a linked candidate feature, keeping the customer's suggested fix under a fixed label.

### 2026-08  Leave product area to triage in the standard mode

A product manager assigns the product at triage.

### 2026-09  Mark change-request features with a field instead of a tag

A structured field that product managers can filter and report on.

### 2026-09  Keep linking insights to features manual

Manual linking in Productboard stays the answer, with a written trigger for revisiting the decision.

### 2026-09  Pick problem themes from a fixed list of 28

Free-text themes had drifted to nearly one distinct value per note.

### 2026-10  Confirm the customer company on the card before filing

The card offers the best matches from Productboard's company list. Filing goes ahead with or without a match.

### 2026-10  Embed the coach in the account ops console through a signed token

Opens in a pop-up window. The host signs a short-lived token, and the coach swaps it for a credential held in page memory.

### 2026-10  Add a host registry and per-host relay keys

Each embedding tool has its own allowed site and key, so one can be revoked alone.

### 2026-10  Strip emails and phone numbers from external users' text

Applied before the model and before filing. Attachments off where host screens could show guest data.

### 2026-11  Hand over, with the voucher admin portal integration awaiting review

The coach and its relay lane were running at handover. The external launch was waiting on review.
