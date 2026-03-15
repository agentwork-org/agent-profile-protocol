# Frequently Asked Questions

---

## General

**What is APP?**

The Agent Profile Protocol is an open standard for describing AI agents
in a machine-readable format. It defines schemas for agent profiles,
client reviews, and job postings — so agents can be discovered, compared,
and evaluated across platforms without vendor lock-in.

**Who created APP?**

APP was authored by Boubacar Diallo and published as a community open
standard in March 2026. It is Apache 2.0 licensed. No single company
controls it.

**Is APP affiliated with Anthropic, Google, or IBM?**

No. APP is an independent community standard. It is designed to
complement MCP (Anthropic) and A2A (Google / Linux Foundation),
not compete with them. See [SPEC.md Section 9](../SPEC.md) for the
A2A compatibility mapping.

---

## Technical

**How does APP relate to A2A AgentCards?**

A2A AgentCards tell other *agents* how to invoke an agent at runtime.
APP profiles tell *human buyers* what an agent does, what its track
record is, and whether to hire it. The two coexist naturally.

An APP profile contains a superset of A2A AgentCard information.
See SPEC.md Section 9 for the field-by-field mapping and how to
auto-generate an A2A AgentCard from an APP profile.

**How does APP relate to MCP?**

APP references MCP in the `integrations.protocol` field — if an agent
uses MCP for tool access, this is declared in its APP profile.
APP does not replicate or replace MCP functionality.

**Do I need to join a registry to use APP?**

No. APP is a schema, not a service. You implement the format in your
own product. No registration, no API key, no approval required.

**What format does APP use?**

YAML or JSON. Both are valid. The JSON Schema files in `/schemas`
can validate any APP object programmatically.

**How do I validate an APP profile?**

```bash
pip install check-jsonschema
check-jsonschema --schemafile schemas/agent-profile.json your-agent-profile.json
```

**Can I add custom fields?**

Yes, using the `x-yourprovider-` prefix:

```yaml
x-acme-internal-score: 94
x-acme-region: "eu-west-1"
```

Custom `x-` fields are ignored by APP-compliant parsers that do not
recognize them. If you believe a field should be in the core spec,
open a GitHub Issue with the label `proposal`.

**What does `trust_level_required` on a skill mean?**

It means the skill only becomes available once the client has granted
the agent that trust level. For example, `trust_level_required: 3`
means the skill is locked until the client explicitly promotes the
agent to Level 3 autonomy. This maps to the agent's `trust_model`
definition.

---

## Reputation & Reviews

**How are reviews verified?**

The `verified: true` flag in a Review Object means the provider
has confirmed via their API that the reviewer was an actual client
during the stated engagement period. Platforms implementing APP
are responsible for their own verification process.

APP defines the format and the `verification_method` field
(`provider_api`, `manual`, or `unverified`). It does not mandate
a specific verification implementation.

**Can a provider manipulate their own reviews?**

Any provider that sets `verified: true` on fabricated reviews is
violating the APP spec and, more importantly, their own clients'
trust. Platforms that aggregate APP profiles may implement
additional verification layers. This is an area for community
extension — see the `a2a-bridge` and future `trust-framework`
discussion threads.

**Are reviews portable?**

That is the goal. A Review Object in APP format can be hosted
anywhere — the provider's own endpoint, a neutral registry,
or the agent marketplace of the client's choice. Portability
requires that the provider exposes their reviews in APP format.

---

## Adoption

**How do I implement APP as a provider?**

1. Describe your agents using the Agent Profile schema
2. Expose profiles at `GET /app/v1/agents` (optional reference API)
3. Enable review submissions and expose them at `GET /app/v1/reviews/{agent_id}`
4. Open a Discussion in the `Implementations` category to list your platform

**I disagree with a field choice in the spec. What do I do?**

Open a GitHub Issue with the label `proposal` or `clarification`.
APP uses a lazy consensus governance model: proposals with no
substantive objections within 14 days are accepted. Your feedback
directly shapes the standard.

**What is the roadmap for APP v1.0?**

APP v1.0 will be the first stable release — meaning no breaking
changes without a new major version. The path to v1.0 requires:

- Community review of the v0.1 draft
- At least 3 independent implementations
- Resolution of open proposals and clarifications
- Formal governance structure (GOVERNANCE.md)

Timeline is community-driven. Watch the GitHub milestones.

---

**Have a question not answered here?**
Open an Issue with the label `question` or start a GitHub Discussion.

*github.com/agentwork-org/agent-profile-protocol*
