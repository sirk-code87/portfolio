# 0006. A downstream refusal degrades a note; it never drops it

## Context

The API of Productboard, the roadmap tool, refuses some inputs: tag values it does not already know, owners it cannot find, users not yet visible straight after creation. (The API is the interface other software uses to write to it.) My first filing code treated any refusal as a failure of the whole note. A cosmetic field could then cost the evidence itself, with no error anyone would see.

## Decision

Every optional part of a note can be removed. On a refusal the relay removes the offending tag, owner or link, retries, and sends an alert naming what was dropped. Only the core note, with the customer's feedback word for word, is required. Fields added later, such as custom fields, follow the same rule: they are dropped and the note still files.

## Alternatives rejected

- Failing the note and sending an alert: the note is still lost, and the alert may land where nobody is looking.
- Checking every tag with Productboard before filing: an extra call per note, and some refusals only appear when the note is written.

## Consequences

- Notes sometimes arrive with less detail than intended. The alerts make that visible and fixable.
- Each new optional field needs its own remove-and-retry path. I first applied the rule to tags only and had to extend it to owners, which taught me to write it as a rule for every field.

## Date

2026-07
