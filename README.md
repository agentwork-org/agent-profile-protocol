# Agent Profile Protocol (APP)

> The registry layer the agentic economy is missing.

**Version:** 0.1 — Public Draft  
**License:** Apache 2.0  
**Status:** Open for community review  

---

## Where APP fits

The agentic ecosystem already has:

- **MCP** (Anthropic) — how an agent accesses tools and data
- **A2A** (Google / Linux Foundation) — how agents communicate with each other

What nobody has built yet:

> *How do you describe an agent so a human can discover, evaluate, and hire it?*

APP is that missing layer.

```
MCP   → agent ↔ tools          (Anthropic)
A2A   → agent ↔ agent          (Google / Linux Foundation)
APP   → agent ↔ human buyer    (open standard — this repo)
```

APP is complementary to both. An agent can expose an A2A AgentCard
for runtime communication *and* an APP profile for discovery,
evaluation, and hiring. They serve different audiences and solve
different problems.

---

## The Problem

Hundreds of companies are building AI agents in 2026.
None can describe their agents in a format anyone else understands.

A business evaluating an AI Executive Assistant from Provider A
cannot meaningfully compare it to one from Provider B.
No common vocabulary. No shared schema. No portable reputation.

- **No standard description** — "expert-level" means something different everywhere
- **No portable reputation** — reviews and track records are siloed per vendor
- **No neutral discovery** — every vendor only surfaces its own agents
- **No standard for trust and autonomy** — no shared model for supervised vs. autonomous agents

APP solves all four.

---

## Quick Example

```yaml
app_version: "0.1"

identity:
  id: "nova-examplecorp-v1"
  name: "Nova"
  role: "Executive Assistant"
  provider: "ExampleCorp"
  status: "available"
  tagline: "Inbox, calendar, standups — handled."

skills:
  - id: "email_triage"
    category: "communication"
    proficiency: "expert"
    tools_required: ["gmail", "outlook"]
    trust_level_required: 1

  - id: "calendar_management"
    category: "scheduling"
    proficiency: "expert"
    tools_required: ["google_calendar"]
    trust_level_required: 1

trust_model:
  type: "progressive"
  levels: 5
  starting_level: 1

pricing:
  model: "subscription"
  currency: "USD"
  plans:
    - name: "Starter"
      price_monthly: 99

reputation:
  tasks_completed: 4821
  avg_rating: 4.6
  rating_count: 18
```

---

## What APP Defines

Three machine-readable objects:

| Object | Purpose |
|---|---|
| **Agent Profile** | Identity, skills, integrations, trust model, pricing, reputation |
| **Review** | Verified client feedback — portable across platforms |
| **Job Posting** | Standardized requirements for agent matching |

APP does **not** define how agents communicate at runtime.
That is A2A's domain. APP defines how agents are **described,
evaluated, and hired by humans**.

---

## A2A Compatibility

APP and A2A solve adjacent problems and are designed to coexist.

An **A2A AgentCard** (at `/.well-known/agent-card.json`) tells
other *agents* how to invoke an agent at runtime.

An **APP profile** tells *human buyers* what an agent does,
what its track record is, and whether to hire it.

A provider implementing APP can generate an A2A-compatible AgentCard
from their APP profile. The `integrations.protocol` field in APP
(e.g., `MCP`, `REST`) maps directly to A2A transport declarations.

Want to contribute a formal APP ↔ A2A bridge spec?
Open an Issue with the label `a2a-bridge`.

---

## Repository Structure

```
agent-profile-protocol/
│
├── README.md                   ← You are here
├── SPEC.md                     ← Full specification
├── CHANGELOG.md                ← Version history
├── CONTRIBUTING.md             ← How to contribute
├── LICENSE                     ← Apache 2.0
│
├── examples/
│   ├── agent-profile.yaml      ← Complete agent profile example
│   ├── review.yaml             ← Review object example
│   └── job-posting.yaml        ← Job posting example
│
└── schemas/
    ├── agent-profile.json      ← JSON Schema (validatable)
    ├── review.json             ← (coming in v0.2)
    └── job-posting.json        ← (coming in v0.2)
```

---

## Validate Your Profile

```bash
pip install check-jsonschema
check-jsonschema --schemafile schemas/agent-profile.json your-agent-profile.json
```

---

## Design Principles

1. **Human-first** — Every agent is tethered to a human or organization
2. **Provider-neutral** — Any provider can implement APP without dependency on any registry
3. **Verifiable** — Reputation claims are provider-verifiable via API
4. **Minimal core, extensible** — Small stable core; use `x-provider-` for custom fields
5. **Privacy-preserving** — No client-identifying data in public profiles

---

## Relationship to Other Standards

| Standard | Owner | Layer | Relation to APP |
|---|---|---|---|
| MCP | Anthropic | Agent ↔ tools | Complementary |
| A2A v1.0 | Google / Linux Foundation | Agent ↔ agent | Complementary — APP is the registry layer A2A lacks |
| SCIM RFC 7643 | IETF | Cross-domain identity | Inspiration for portable profile design |
| OpenAPI | OpenAPI Initiative | API description | Structural inspiration |

---

## Current Status

**Version 0.1 — Public Draft.**
Breaking changes may occur before v1.0.
Stable release timeline is community-driven.

- [Read the full spec](./SPEC.md)
- [Open an issue](../../issues)
- [Start a discussion](../../discussions)
- [See examples](./examples)

---

## Contributing

Proposals → GitHub Issues. No objections in 14 days → accepted.
Major changes → GitHub Discussion, 30-day comment period.
See [CONTRIBUTING.md](./CONTRIBUTING.md).

---

## License

Apache License 2.0 — free to implement in any product,
open or closed source, with attribution.

---

*APP is a community standard. Contributions welcome.*
*github.com/agentwork-org/agent-profile-protocol*
