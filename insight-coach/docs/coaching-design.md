# Coaching design

The coach exists to produce one thing: a product insight a product manager can triage (sort and prioritise) without going back to the person who raised it. This document describes that insight, the questions that build it and the rules that keep it within what the raiser said. The rules are written fresh for this repository; field names are mine.

## The shape of a coached insight

A filed insight has four layers, and each has a different owner.

| Layer | Contents | Who writes it | What the coach may change |
|---|---|---|---|
| The raiser's words | Every message the raiser typed, joined in order | The raiser | Nothing. Stored exactly as typed. |
| The framing | A title and a problem statement | The coach drafts, the raiser edits | Everything, until confirmation |
| The classification | Evidence tier, how it arrived, signal kind, problem theme | The coach proposes within fixed rules | Only what the rules below allow |
| The attribution | Customer, raiser, surface | Customer confirmed by the raiser; raiser and surface set by the server | The customer suggestion only |

The split matters because the framing is an interpretation. A product manager reading a title that says "group revenue manager rebuilds a report by hand" can always check it against what the raiser typed, because that text is kept exactly as typed.

## Question flow

The coach asks up to three questions. Each one fills a specific gap in the insight, and the coach skips any question already answered in the conversation.

| # | Question, in substance | Why it exists | Skipped when |
|---|---|---|---|
| 1 | What problem sits behind this request? What is the customer trying to get done? | Most requests arrive as a solution. The problem is what a product manager can compare across customers; the solution is one person's guess at a fix. | The first message already describes a problem. |
| 2 | Did the customer say this, did you see it, or is it your read? | This answer sets the evidence tier, so a product manager can tell strong evidence from a hunch. | The raiser has already quoted the customer or described seeing it. |
| 3 | How often does it happen, or what does it cost them? | Frequency and impact separate an irritation from a blocker. | Either is already stated, or the raiser doesn't know. Not knowing is a valid answer and the field stays empty. |

Behaviours around the questions:

- **File as-is, from the first turn.** The raiser can stop at any point and go straight to the card. Keeping filing open at every turn keeps people using the coach.
- **Pasted material.** If the raiser pastes a transcript or a long email, the coach finds the problem in it before asking anything.
- **One conversation, several insights.** If the raiser describes two problems, the coach drafts two insights. An observation that covers several customers becomes one insight per customer.
- **Product chosen at triage.** A product manager assigns the product. I first moved that choice out of the chat onto the card, then removed it from the standard mode.
- **Too terse to work with.** A message under a minimum length is the one case where the coach asks for more before filing.

## Evidence tiers

| Tier | Meaning | Example |
|---|---|---|
| Quote | The customer's own words, as the raiser heard or read them | "I lose Monday mornings to this." |
| Observation | The raiser saw it happen | "I watched her export five reports and paste them together." |
| Inference | The raiser's read, without direct evidence | "I think their group team struggles with reporting." |

Tiers only round down. The rules:

1. The model proposes a tier. Code applies a cap, and the filed tier is the lower of the two.
2. An unrecognised label becomes inference, the weakest tier. Recognised synonyms map to their tier ("hunch" to inference, "verbatim" to quote).
3. Quote requires the quoted words to appear in what the raiser typed. Otherwise the tier drops to observation.
4. On the card, the raiser can lower the tier. To raise it, the raiser edits the evidence text and the check runs again, because a higher tier means the coach under-read the evidence.

In the original, code enforced rules 1 and 2. Rules 3 and 4 are the stricter checks I'd add; the first version relied on the model to tell a quote from a paraphrase.

Rounding down is a deliberate bias. An insight filed as inference that was really a quote costs a product manager a follow-up question. An insight filed as a quote that was really a guess distorts prioritisation, and nobody notices.

## How it arrived

Every insight carries a flag recording the shape of the raiser's first message:

- **Solution**: it named a fix ("add a bulk export").
- **Problem**: it described a difficulty ("their revenue manager rebuilds the report by hand").
- **Too terse**: there was too little to tell ("bookings report?").

The flag comes from the first message only. By the time the card appears, the coach has turned the request into a problem statement, so the filed text reads problem-first whatever the starting point. The first message shows where the raiser began. That makes the flag polish-proof: a request run through a general AI assistant first reads better and still shows up as a solution.

If the coach cannot tell, it leaves the flag out. An absent flag is honest; a guessed one would corrupt the only measure of whether people's framing is changing.

## The raiser's words stay verbatim

Everything the raiser typed is stored as one field, in order, exactly as typed. The coach writes only the title and the problem statement. In the original, the code followed this as a rule. The stricter version has the server compare the stored text with the raiser's messages and refuse any mismatch. Every claim in the framing can then be traced back to the words it came from.

## Anti-laundering rules

"Laundering" here means a draft that sounds stronger than the evidence behind it. The coach makes a request clearer and keeps it exactly as strong as the evidence. Every specific in the draft comes from the raiser:

- **Numbers come from the raiser.** If the raiser says "most of a morning", the draft says "most of a morning". The coach never converts it into "four hours".
- **Causes come from the raiser.** The coach may ask why something happens. The reason in the draft is the one the raiser gave.
- **Customers come from the raiser.** The customer comes from what the raiser said and the company they confirm on the card. A missing customer stays unknown.
- **Problems describe the need.** "The customer lacks a bulk export" is a solution with a problem's grammar. The problem statement describes what the customer is trying to do and what gets in the way.
- **Evidence keeps its tier.** The tier rules above apply.

The promise to the raiser was short: what's filed is what you confirmed.

## Confirm before filing

The card shows every field that will be filed, classification and attribution included. The raiser corrects anything wrong, then confirms explicitly to file. In a stricter version, the filing request carries a hash (a short fingerprint of the data) of exactly what the card displayed, and the server files only data that matches it.

## Attribution

An insight names two parties.

- **The customer**: the company the feedback came from. The card offers the best few matches from Productboard's company list and lets the raiser pick or search. Filing goes ahead either way: an unmatched insight files without a company and is flagged for a product manager.
- **The raiser**: set by the server from the signed-in identity, overwriting anything the browser sends. The relay links the raiser to the note as its creator, so they can follow what happens to it.

The server also sets the surface (web, Slack or the embedding tool), so a product manager sees where insights come from based on a value the server controls. The detailed attribution model, including note ownership, is in the relay's [decision record](../../insight-relay/docs/decisions/0005-attribute-to-customer-and-raiser.md).

## Payload

A generic version of the data (the payload) the coach sends to the relay after confirmation.

| Field | Type | Set by | Rule |
|---|---|---|---|
| `title` | text, 1 to 120 characters | Coach, edited by raiser | Problem-first; shortened at a word boundary |
| `problem_statement` | text | Coach, edited by raiser | Describes the difficulty; every specific comes from the raiser |
| `raw_text` | text, minimum length | Raiser | Verbatim; server checks it matches the raiser's messages |
| `evidence_tier` | quote, observation, inference | Coach within caps | Lower of model and code; unknown becomes inference |
| `arrived_as` | solution, problem, too_terse, or absent | Coach | First message only; absent if unsure |
| `signal_kind` | idea, stakeholder request, discovery | Coach | Unrecognised values fall back to discovery |
| `heard_via` | fixed list: call, email, visit and similar | Coach, from the conversation | Where the customer said it; taken from what the raiser wrote |
| `customer` | company reference or unknown | Raiser, on the card | Optional at filing |
| `problem_theme` | one of a fixed list, or other | Coach | See [0007](decisions/0007-fixed-problem-themes.md) |
| `who_affected` | text, optional | Coach | Only if the raiser said it |
| `commercial_signal` | fixed list, optional | Coach | Only if the raiser said it |
| `raised_by` | identity | Server | From the session, overwriting any browser value |
| `surface` | web, slack, embedded tool | Server | Set by the server alone |

## A variant for change requests

The customer success team used a separate mode for formal change requests from customers. Access was limited to a named list of that team, and anyone else was refused by default. In this mode the coach splits a customer's change request into the problems inside it. It keeps the customer's suggested fix, and their answer on whether another approach would work, under fixed labels. Each submission filed a note plus a linked candidate feature that a product manager could prioritise.

## After filing

The completion screen says where the insight goes: into a pool of evidence that product managers read for patterns. It states that individual replies are rare by design and offers a way to book time with the product team for anything urgent. That screen was my answer to the trust problem. Closing the loop to each raiser needs a link from each insight to the feature that resolves it.
