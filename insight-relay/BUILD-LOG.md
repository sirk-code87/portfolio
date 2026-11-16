# Build log

Reconstructed from my own dated notes on the original, private repositories.

The **direct lane** is a webhook: a web address other tools send a single note to, which files it at once and sends back a link. The **judged lanes** run on a schedule and use an AI model as a judge to decide what to file. A lane in **dry-run** is in test mode: it posts what it would have filed and files nothing.

### 2026-06  Add a direct lane so staff can file notes without a roadmap-tool login

A Claude Code skill (a packaged command for Anthropic's coding assistant) needed to file Productboard notes for staff who had no Productboard login. The relay held the one access key to Productboard's API, the interface other software uses to talk to it.

### 2026-06  Replace the survey feed that filed every response with a daily judged run

Filing every survey response buried the few that named a real problem. A scheduled AI judge now picks out items that describe a specific problem.

### 2026-06  Add a scheduled filing step that matches companies and posts a Slack summary

The judge writes a batch; a separate filing step matches each note to its company, files the notes and posts a summary with links. Writing straight to Productboard from the scheduled job was harder to recover when it failed, so I dropped it.

### 2026-06  Match companies only on words that identify exactly one company

Loose name matching attached feedback to the wrong customers. Only a word that identifies exactly one company now counts as a match.

### 2026-07  Hold survey answers that propose a fix until a second customer raises the topic

One customer's suggested fix says little on its own. When a second, different customer raises the topic, it files.

### 2026-07  Add a call-transcript lane to the shared relay, starting in dry-run

Product gaps only, with a short quote as evidence and names removed. Raw transcripts are never stored. Items that fail to file stay off the list of filed items, so the next run retries them.

### 2026-07  Move the daily customer-list sync from the CRM into the relay

A daily scheduled job on Vercel copies customer companies from the CRM (the customer relationship system) to Productboard. It only updates existing records, rebuilds its company map each run and keeps nothing between runs. It keeps every customer company current.

### 2026-07  Add a support-desk lane on the same scheduled pattern

Weekly, with a high bar for what counts as a product problem. I set that bar from my item-by-item grades of the dry-run summaries.

### 2026-07  Run automatic tests on the direct lane before changes merge

Shared filing code is now tested before it reaches production, through CI (continuous integration, checks that run on every proposed change).

### 2026-07  Drop a refused tag and keep the note

When Productboard refuses a tag, the relay now removes it, files the note and raises an alert.

### 2026-07  Record who raised each note as a Productboard user and take the name out of the note text

Each person's link to their notes sits in one record that can be deleted in one call.

### 2026-08  Move the scheduled lanes onto GitHub Actions, with the Claude API as judge

GitHub Actions is GitHub's service for running jobs on a schedule. The move takes the lanes off my personal account before handover, with API keys and a schedule the team owns.

### 2026-08  Apply drop-and-alert to note owners as well as tags

When Productboard refuses a note's owner, the relay removes it, retries and alerts, as it already did for tags.

### 2026-09  Send each lane's messages to its own Slack channel and add error tracking

Each lane posts to its own channel; failures go to a shared alert channel.

### 2026-09  Let new automated channels file without approving each note

Each new channel gets a scheduled judge, a dry-run period and a graded summary before it goes live. Approving each note stays with the coach, where a person is present.

### 2026-09  Tag every note with its source from a fixed list keyed by stable IDs

Tags built from survey titles broke when a title changed. The new tags come from a list keyed by IDs that never change. Rolled out across five lanes.

### 2026-09  Add an in-app feedback lane that judges in two stages and removes personal details, in dry-run

Removes guest names and booking references, and keeps what a product manager needs to judge the problem.

### 2026-10  File staff notes under the customer and add a shared company cache

The customer is confirmed on the coach's card. I updated every existing note with a clear customer, with no failures. Company lookup went from about 12 s on a cold start to 0.5 s.

### 2026-10  Give each caller its own key and check keys securely

Keys are compared in constant time, a check that reveals nothing through how long it takes. Each caller can now be revoked on its own, ready for the coach built into internal tools.

### 2026-10  Add an optional Slack feed for each tool that files notes, off by default

Summary and link only, never the raw conversation. Built once in the relay for every tool.

### 2026-11  Hand over all lanes, with owners to be assigned

All lanes were running at handover, with a handover pack covering schedules, credentials and each judge's written charter.
