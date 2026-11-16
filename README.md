<p align="center">
  <img src="assets/banner-portfolio.svg" alt="Portfolio: four product systems I designed and shipped, and the product decisions behind them." width="100%">
</p>

<p align="center">
  <a href="pm-agent-os/">pm-agent-os</a> ·
  <a href="insight-coach/">insight-coach</a> ·
  <a href="insight-relay/">insight-relay</a> ·
  <a href="account-ops-console/">account-ops-console</a> ·
  <a href="product-case-studies/">case studies</a>
</p>

Systems I designed and shipped as Head of Product at a UK hospitality commerce company serving luxury hotels and spas (2026), and product decisions from there and from a restaurant ordering and payments platform before it (2025). The original systems and their code belong to those companies. Everything here is my own write-up, with customers, partners and colleagues by role only.

## Systems

| | System | What it is | Read |
|---|---|---|---|
| 🧭 | [**pm-agent-os**](pm-agent-os/) | The AI agent system I ran a product leadership role on. Nine specialist lenses, a weekly report built only from logged facts, and a gate on every outgoing message. | [Architecture](pm-agent-os/docs/architecture.md) · [Decisions](pm-agent-os/docs/decisions/) · [Build log](pm-agent-os/BUILD-LOG.md) |
| 💬 | [**insight-coach**](insight-coach/) | A two-to-three question chat that turns "something I noticed" into an evidenced product insight. | [Architecture](insight-coach/docs/architecture.md) · [Decisions](insight-coach/docs/decisions/) · [Build log](insight-coach/BUILD-LOG.md) |
| 🔀 | [**insight-relay**](insight-relay/) | One intake for about ten feedback sources, with an AI judge and a trial run before any automated lane files to the roadmap. | [Architecture](insight-relay/docs/architecture.md) · [Decisions](insight-relay/docs/decisions/) · [Build log](insight-relay/BUILD-LOG.md) |
| 💳 | [**account-ops-console**](account-ops-console/) | An incident tool for creating Stripe accounts that grew into the operations team's console for every merchant payment account. | [Architecture](account-ops-console/docs/architecture.md) · [Decisions](account-ops-console/docs/decisions/) · [Build log](account-ops-console/BUILD-LOG.md) |

Every system follows the same shape: **problem → who it was for → what it does → decisions → build log → what I'd do differently.** The build logs are written up from dated decision records and include the reversals.

## Product case studies

Each study runs problem, solution and impact: how I found the problem, how I chose the fix, and what it measurably changed.

- [**Retuning fraud rules that were turning away good guests**](product-case-studies/fraud-rules-blocking-good-guests/): a reversible call made live when no quick test could prove it safe, measured against triggers agreed in advance.

More studies on prioritisation, payments and integrations are being added.

## How the pieces fit

```mermaid
flowchart LR
    signals["What people notice<br/>colleagues, surveys, calls,<br/>tickets, CRM"]
    coach["insight-coach"]
    relay["insight-relay"]
    pb[("Roadmap<br/>Productboard")]
    runner["Overnight agent run<br/>drafts the change"]
    human{{"An engineer reviews<br/>and approves"}}
    console["account-ops-console"]
    os["pm-agent-os"]

    signals --> coach --> relay --> pb --> runner --> human --> shipped(["Fix goes live"])
    signals --> relay
    console -->|"feedback button"| coach
    os -.->|"runs"| relay
    os -.->|"runs"| runner
    os -.->|"reports on"| pb

    classDef mine fill:#134e4a,stroke:#2dd4bf,stroke-width:2px,color:#f8fafc
    classDef gate fill:#fbbf24,stroke:#b45309,color:#0b1220
    class coach,relay,console,os mine
    class human gate
```

## Reading notes

- Case studies give elapsed time ("at six weeks") rather than calendar dates.
- Diagrams are Mermaid and render on GitHub.
- Stripe, Productboard, Claude Code and Vercel are named because the work is specific to them. Other vendors are described generically.
- Docs only: the code stayed with the employers.
