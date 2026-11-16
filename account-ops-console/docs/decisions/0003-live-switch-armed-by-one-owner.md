# 0003. A live switch that only I can arm

**Date:** 2026-10

## Context

The console was built and tested against Stripe's sandbox, its test environment. Live writes needed a deliberate step, and that step needed an owner who would answer for it.

## Decision

Live creates and onboarding links check a live-mode setting before calling Stripe. Only accounts on a short configured list, in practice me, can arm (turn on) the setting. Any admin can disarm it. Reads run without the switch; the key itself is the control for reads.

## Alternatives rejected

- **Any admin arms it with a free-text justification.** The control is who acts, and a free-text field leaves that unchanged.
- **A setting fixed at deploy time.** Disarming would then need a deploy, which is too slow when something goes wrong.

## Consequences

- Turning live writes off is fast and open to any admin; turning them on is slow and personal.
- It concentrates authority in one person. That suited an incident tool and became a weakness once the tool grew, because I was both the person proposing live use and the person approving it.
