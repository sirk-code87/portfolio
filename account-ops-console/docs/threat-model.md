# Threat model and privacy

A threat model lists what could go wrong with a system and the control for each risk. This one is generic, written fresh for this repository, and describes the approach I use for this kind of tool: an internal operations console on a Stripe Connect platform (one business taking payments on behalf of many others, each with its own connected Stripe account). Staff view connected accounts and send onboarding or update links to account holders.

It uses STRIDE, a standard checklist of six threat types: Spoofing (pretending to be someone else), Tampering (changing data or settings), Repudiation (denying an action took place), Information disclosure (leaking data), Denial of service (stopping people using the system) and Elevation of privilege (gaining powers beyond your role).

## Assets

- The ability to send an account holder into a Stripe onboarding session, where details such as bank accounts can change.
- The platform's restricted Stripe key.
- The mapping from merchant to connected account.
- The audit trail.

On platforms where the platform carries losses for connected accounts, a takeover through a misdirected onboarding link is the threat that matters most.

## Threats and controls by STRIDE category

| Category | Threat | Controls |
|---|---|---|
| Spoofing | Link reaches the wrong person | Send only to the contact of record in the CRM; short expiry; revocation; one live link per account; links that expose bank details restricted to admins |
| Spoofing | Token guessing | 256-bit random tokens stored only in hashed (one-way scrambled) form; format check before lookup; rate limits per IP address and per token |
| Tampering | Bank details changed through an onboarding session on a live account | Verify in Stripe test mode which fields a session exposes when nothing is due; stop the feature if it exposes more than intended |
| Tampering | Wrong merchant-account mapping sends a link for the wrong account | Links only for accounts with a confirmed mapping; a mismatch threshold on a sample check before launch |
| Repudiation | Staff deny sending a link, or a merchant denies requesting an account | Append-only audit log written in the same database step as the action, stopping the action if the log write fails; the public page records the merchant's acknowledgement before redirecting to Stripe |
| Information disclosure | Tokens appear in hosting logs or Stripe's refresh URL | Short expiry and one live token per account limit the window; record as an accepted risk if unavoidable |
| Information disclosure | Error pages reveal why a link failed | Generic message to the visitor; the reason goes to the audit log only |
| Denial of service | Link-preview bots (tools in chat and email apps that open links to show a preview) use up single-use Stripe links | Opening the public page calls nothing in Stripe; a Stripe link is created only when the visitor clicks Continue |
| Elevation of privilege | An insider targets particular accounts | Per-user and global caps on links issued per day; a Slack post for every link; regular review of the audit log |
| Elevation of privilege | Key misuse through an unexpected code path | Each feature gets a narrow gateway port (a small interface) exposing only the calls it needs; live writes require a separately armed switch |

## Key design principles

- Use a restricted key with only the permissions the features use. Add Charges read (access to Stripe's record of each card payment) only when a feature needs cardholder-adjacent data.
- Two keys in one deployment give little separation, since anything that can read one can read the other. Use separate keys when they live in separate deployments.
- Gate live writes behind a switch that a named owner arms and any admin can disarm.
- Map every Stripe object to a narrow internal type at the gateway and drop the rest, so new fields stay out of storage and logs by default.

## Accepted departures from Stripe's guidance

Stripe advises against sending account link URLs to users, because they are single-use and short-lived. An ops console that emails a merchant needs something that survives until the merchant opens it. The approach here is a stable signed URL on the platform's own domain that creates a fresh Stripe link only when the merchant clicks Continue. That is a deliberate departure, and it should be recorded as a decision with its risks.

## Privacy approach

- **Minimise at source.** Bank details, person records, personal contact details, dates of birth, cardholder data and end-customer data all stay in Stripe. Shorten IP addresses when writing them.
- **Public notice.** A short privacy page on the public route stating what is collected and for how long, with no cookies or analytics.
- **Staff notice**, plus a warning in any free-text feedback box about what to leave out.
- **Screening.** Run a data protection impact assessment screening (a short check for privacy risk), and re-run it whenever a new kind of personal data is added.
- **Contact data.** Keep merchant contacts out of audit rows, error reports, chat alerts, exports and logs. An append-only audit log makes any leak into it permanent.
- **Retention.** Set a deletion rule for contacts of merchants who never complete setup.

## Operational threats to add

STRIDE covers attackers and misuse. A console can also go quiet with no attacker involved. Add two rows when you adapt it: monitoring that runs independently of the app's database, and environment separation that keeps development away from live data. I learned both on this console; see "What I'd do differently" in the [README](../README.md).
