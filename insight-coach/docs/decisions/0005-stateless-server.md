# 0005. The conversation lives in the browser, and the server keeps no database

## Context

Conversations contain customer details and colleagues' frank opinions. Productboard already held the filed insight as the system of record. Storing chats on the coach's side would have created a second store of personal data to secure, retain and delete, for a tool whose output lived somewhere else.

## Decision

- The browser holds the conversation and sends the full history with each request.
- The server keeps nothing between requests, so no personal data rests on the coach's side.
- Identity comes from the signed-in session on every request, and the server overwrites any identity the browser supplies.
- The Slack front-end is the exception. Slack delivers each message as a separate event, so it keeps short-lived conversation state in a simple lookup store (a key-value store).

## Alternatives considered

- A database of conversations: useful for analysis, and a liability with no owner once the insight is filed.
- Server-side sessions: on serverless hosting, where each request may reach a different short-lived server, this needs either pinning each user to one server or a shared store, for no gain to the raiser.

## Consequences

- Any server instance can answer any request, which suits serverless hosting.
- A page refresh loses an unsent draft. I accepted that and documented it for users.
- Retention questions for the coach have a short answer: it keeps nothing. Retention lives with the Productboard note.
- Per-user rate limits were held in each server's memory, so they were approximate. A durable limit would need the shared store this decision avoided.
- Usage measurement relied on anonymous analytics events and counters, since conversations stayed in the browser.

## Date

2026-07
