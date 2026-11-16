# 0001. Build the incident tool as a standalone app

**Date:** 2026-10

## Context

Implementations needed a way to create Stripe connected accounts while the Stripe Dashboard route was failing. The tool needed a live key with permission to create accounts. The obvious homes for it were an existing internal dashboards app, which many staff could reach, or an existing payments service, which had a short request time limit and lacked a role suited to ops staff.

## Decision

I built a standalone web app (Next.js, hosted on Vercel), modelled on the existing dashboards app so engineers would recognise its structure, with its own invite list and its own restricted key, limited to named permissions.

## Alternatives rejected

- **An engineer runs a script per property.** It works for a handful of properties, but ties an engineer to every install for as long as the incident lasts.
- **A page in the internal dashboards app.** It would put a live payments write key inside an app with much wider access.
- **A function in the payments service.** The create plus find-with-retries flow needed longer than its time limit allowed.

## Consequences

- The key's exposure is limited to one small app with a short invite list.
- A new app needs its own hosting, database, monitoring and deploy path. I set up hosting and the database properly and launched without monitoring; the console's later operational failures came from that gap.
