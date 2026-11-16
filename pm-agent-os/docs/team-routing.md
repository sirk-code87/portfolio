# Team routing

`/team` is the command that sends a question or a task to the nine persona lenses: role-based viewpoints, each run by an AI agent. It began as a roster in August 2026 and became a dispatch table in September, used by the handoff runner, persona skills, task sweeps and ad hoc sessions.

The original table was lost when the private configuration was deleted at handover. This version is rebuilt from how sessions recorded the router's choices. It is written as an explicit table of what each lens owns, with a "primary when" column. That is the logic the sessions show, gathered in one place for the first time.

## Lens ownership

| Lens | Owns | Primary when | Usual contributors |
|---|---|---|---|
| Product | Value and viability: what problem, for whom, on what evidence, and whether it is worth doing | The output has to combine several lenses into one recommendation: market entry, a strategy test, logging a feature. Default primary when nothing else binds. Tiebreaker of record in decision reviews. | Any lens whose question binds |
| Engineering | Feasibility: architecture, code readiness, hosting, effort. Settles technical claims against the code itself. | Feasibility is the binding constraint: hosting, data residency, a repository fix | Legal when a regulatory perimeter applies |
| Commercial | Customer health, churn and retention impact, customer voice, deal and pricing qualification | Deal or account qualification. My notes hold no session where it led, so this row is inferred from its skills. | Product, finance |
| Operations | How work runs: meetings, rhythms, ownership, coordination, handover | Process and cadence questions | Product |
| Design | Usability and visual quality of screens, documents and diagrams | A screen, page or document critique | Product, marketing |
| Marketing | Positioning, audience, go-to-market, competitor and pricing scans | Competitive and pricing research | Finance |
| Legal | Contract, regulatory, privacy and compliance perimeter | A perimeter question decides the answer | Engineering, product |
| Data | Measurability: metric definitions, denominators, baselines, sample size; whether a claimed number holds | A metric or a claimed figure is in question | Product, finance |
| Finance | Unit economics, models, budgets, the money case. Holds a veto when the economics fail. | Building or checking a model | Data, product |

Sessions show product, engineering and marketing as primaries directly. The other "primary when" entries follow from what each lens owns.

## Routing rules

1. **One to three lenses, one primary.** Pick the fewest lenses that own a binding sub-question, name a primary, and cap the panel at three.
2. **The primary integrates.** Contributors supply their lens; the primary writes the single answer and states where contributors disagreed.
3. **Route by ownership.** Split the request into sub-questions and send each to the lens that owns it in the table above, whoever asked.
4. **Binding inputs first.** For a requirement change, gather the constraints (engineering reality, a compliance limit) before product integrates.
5. **Inline is a valid outcome.** If one lens covers it and the work is small, do it in the main session (the conversation I am in) and skip the brief.
6. **Decision review runs in parallel.** For reviewing a decision, run the relevant lenses side by side with product as tiebreaker of record. I can override the panel, and the record then says it was an override.
7. **Delegates are draft-only.** Delegates are the helper agents that carry each lens. They return text. They write no files and take no outward action.
8. **Brief fully, once.** Each delegate gets the task, linked context, the expected outcome, what is already established, and its persona's lesson digest (a short list of past corrections).
9. **Questions come back to the hub.** Delegates return open questions with their draft; the main session collects them after all agents return and routes them on.
10. **Doors decide autonomy.** A two-way door is a decision that is easy to undo; a one-way door is hard to undo. Delegates may proceed on two-way doors by stating a proposed default. One-way doors (writes to systems of record, new channels, secrets, merges, deploys) are stop points until I authorise.
11. **Input is not agreement.** Decision records list persona input separately from the decider.
12. **Re-brief a stalled delegate.** If a delegate wanders into exploration, dispatch again with a tighter prompt that forbids it.
13. **Tasks may name an owner lens.** A task that states its owner lens is routed there first.

## Outside the nine

- **Stakeholder-appetite lens.** A sparring skill that models how a senior stakeholder weighs risk and appetite, used to pressure-test a call before taking it to the real person. Its output is labelled simulated and ends in one question to ask them. Publish it only as a template; a model of a named individual stays private.
- **Thinking lenses** for strategic sparring and blind parallel critique, used without routing.

## Evidence and limits

- At least 14 sessions between late August and late October record `/team` routing, and one task sweep ran 22 agents over about 135 open tasks, grouped by lens.
- Lenses disagreed usefully: in one session the data lens found an analytics event that answered a question the product lens had called unanswerable.
- Lenses also missed things: a design review missed duplicated controls that I caught from screenshots.
- Not yet measured: which lens choices were right and how many drafts I kept, because the router logged neither. A rebuild should log the lenses chosen and whether I accepted the output, from the first run.
- What changed in the October revision of `/team` is not in my notes.
