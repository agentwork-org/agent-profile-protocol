# Agent Profile Protocol (APP)
## Version 0.1 — Public Draft Specification

**Status:** Draft — Community Review  
**Authors:** Boubacar Diallo  
**License:** Apache 2.0  
**Repository:** github.com/agentwork-org/agent-profile-protocol  
**Published:** March 2026  

---

## Abstract

The Agent Profile Protocol (APP) is an open standard for describing
AI agents in a machine-readable, interoperable format. APP enables
AI agents to be discovered, compared, evaluated, and integrated across
platforms — without vendor lock-in.

APP addresses a critical gap in the agentic ecosystem. The industry
already has MCP (Anthropic) for agent-to-tool communication and A2A
(Google / Linux Foundation) for agent-to-agent communication. What
is missing is a standard for how humans discover, evaluate, and hire
agents. APP is that layer.

---

## 1. Motivation

### 1.1 The Problem

As of 2026, hundreds of companies are building AI agents. Each
platform describes its agents in proprietary formats. A business
evaluating an AI Executive Assistant from Provider A cannot
meaningfully compare it to one from Provider B — there is no common
vocabulary, no shared schema, no interoperable description format.

This creates four structural problems:

**1. No portability.** An agent deployed on Platform A cannot be
described, evaluated, or migrated using a standard format understood
by Platform B.

**2. No portable reputation.** Performance data and reviews are
siloed within each platform. An agent's track record does not travel
with it.

**3. No neutral discovery.** Every vendor only surfaces its own
agents. There is no neutral place to compare agents across providers.

**4. No standard vocabulary.** Terms like "autonomous," "supervised,"
"expert-level," or "trusted" mean different things on different
platforms. Buyers cannot make informed decisions.

### 1.2 The Ecosystem Context

The agentic protocol landscape in 2026:

| Protocol | Owner | Layer | Problem Solved |
|---|---|---|---|
| MCP | Anthropic | Agent ↔ tools | How an agent accesses tools and data |
| A2A v1.0 | Google / Linux Foundation | Agent ↔ agent | How agents communicate at runtime |
| **APP** | **Community (this spec)** | **Agent ↔ human buyer** | **How humans discover, evaluate, hire agents** |

APP is explicitly designed to complement, not compete with, MCP and A2A.
See Section 9 for the A2A compatibility bridge.

### 1.3 The Precedent

The internet has solved this class of problem repeatedly:

| Problem | Solution | Year |
|---|---|---|
| Email interoperability | SMTP (RFC 821) | 1982 |
| Web content | HTML (W3C) | 1993 |
| API description | OpenAPI Specification | 2011 |
| Cross-domain identity | SCIM (RFC 7643) | 2015 |
| Package metadata | npm, PyPI manifests | 2010s |

AI agents need their equivalent of a universal profile format.
APP is that format.

### 1.4 Why Now

Major platforms are beginning to build proprietary agent registries.
APP proposes the open alternative, ensuring no single company controls
how agents are described and discovered. An open standard benefits
the entire ecosystem.

---

## 2. Design Principles

**1. Human-first.** Every agent is associated with a human or
organization. There are no ownerless agents in APP.

**2. Provider-neutral.** APP describes agents, not platforms. Any
provider can implement APP without dependency on any specific registry.

**3. Verifiable.** Claims in a profile (tasks completed, ratings,
badges) should be verifiable via the issuing provider's API.
APP distinguishes between unverified and provider-verified data.

**4. Minimal core, extensible.** The core schema is small and stable.
Advanced fields are optional. Providers may extend the schema using
`x-provider-` namespaced fields without breaking compatibility.

**5. Privacy-preserving.** Agent profiles contain no client-identifying
data. Reputation metrics are aggregate and anonymized. Individual
client details are never part of a public profile.

---

## 3. Core Schema

APP defines three primary objects:

- **Agent Profile Object** — Describes an AI agent
- **Review Object** — Describes a verified client review
- **Job Posting Object** — Describes a requirement for an AI agent

All objects include an `app_version` field for forward compatibility.

---

### 3.1 Agent Profile Object

```yaml
# APP v0.1 — Agent Profile Object

app_version: "0.1"

identity:
  id: "nova-examplecorp-v1"
  # Globally unique identifier. Recommended format: {name}-{provider}-{version}
  # Must be lowercase, hyphen-separated, URL-safe.

  name: "Nova"
  # Display name of the agent.

  role: "Executive Assistant"
  # Primary role. Free text, max 64 characters.
  # Examples: "Executive Assistant", "Sales Development Rep",
  #           "Data Analyst", "Customer Support Agent"

  provider: "ExampleCorp"
  # Name of the company or individual that built and operates this agent.

  provider_url: "https://example-provider.com"
  # Canonical URL for the provider.

  provider_agent_url: "https://example-provider.com/agents/nova"
  # Direct URL to this agent's page on the provider's platform.

  version: "1.0"
  # Agent version. Providers increment on significant updates.

  created_at: "2026-01-01"
  # ISO 8601 date when this agent profile was first published.

  status: "available"
  # Allowed values: available | unavailable | deprecated

  avatar_url: "https://cdn.example-provider.com/nova.png"
  # Optional. Publicly accessible image URL. Recommended: 256x256 PNG.

  tagline: "Senior EA. Inbox, calendar, standups — handled."
  # One-line description. Max 128 characters.

  owner_type: "provider"
  # Who operates this agent on behalf of clients.
  # Allowed values: provider | individual | organization


personality:
  communication_style: "professional, warm, proactive"
  # Free text. Describes the agent's general interaction style.

  languages:
    - "en"
    - "fr"
  # ISO 639-1 language codes.

  tone_tags:
    - "direct"
    - "concise"
    - "structured"
  # Optional. Short descriptors for tone. Used for search and filtering.


skills:
  - id: "email_triage"
    # Unique skill identifier within this agent's profile. Use snake_case.

    category: "communication"
    # Recommended values:
    # communication | scheduling | research | writing |
    # coding | data-analysis | finance | legal | customer-support | custom

    proficiency: "expert"
    # Allowed values: beginner | intermediate | expert

    description: "Classify inbox by urgency and topic, draft replies, detect time-sensitive items."
    # Free text. Max 256 characters.

    tools_required:
      - "gmail"
      - "outlook"
    # Tools must also appear in integrations.supported.

    trust_level_required: 1
    # Minimum trust level at which this skill becomes available. Integer 1-5.

  - id: "calendar_management"
    category: "scheduling"
    proficiency: "expert"
    description: "Schedule meetings, detect conflicts, prepare agendas, send reminders."
    tools_required:
      - "google_calendar"
      - "outlook_calendar"
    trust_level_required: 1

  - id: "meeting_notes"
    category: "documentation"
    proficiency: "expert"
    description: "Generate structured notes, extract action items, distribute summaries."
    tools_required:
      - "google_docs"
      - "notion"
    trust_level_required: 2

  - id: "autonomous_outreach"
    category: "communication"
    proficiency: "intermediate"
    description: "Send communications without per-message approval."
    tools_required:
      - "gmail"
    trust_level_required: 3
    # Higher trust_level_required signals a sensitive capability.


integrations:
  protocol: "MCP"
  # Integration protocol used by this agent.
  # Allowed values: MCP | REST | custom | none
  # MCP = Anthropic Model Context Protocol
  # This field maps to A2A transport declarations for cross-protocol compatibility.

  supported:
    - name: "slack"
      status: "native"
      # native    = fully supported, maintained by provider
      # community = supported via third-party integration
      # coming_soon = planned, not yet available

    - name: "google_workspace"
      status: "native"

    - name: "microsoft_teams"
      status: "coming_soon"

    - name: "notion"
      status: "community"


trust_model:
  type: "progressive"
  # Allowed values:
  #   progressive — agent starts supervised, earns autonomy over time
  #   flat        — agent operates at a fixed autonomy level
  #   custom      — provider-defined model, see description field

  levels: 5
  # Number of trust levels. Integer 1-5.

  starting_level: 1
  # Trust level at which new clients begin.

  description: >
    Starts fully supervised at Level 1. Earns autonomy through
    consistent task completion and low revision rates. Levels 1-3
    are auto-promoted based on performance. Levels 4-5 require
    explicit client approval.


pricing:
  model: "subscription"
  # Allowed values: subscription | per_task | per_hour | custom

  currency: "USD"
  # ISO 4217 currency code.

  plans:
    - name: "Starter"
      price_monthly: 99
      compute_budget_hours: 30

    - name: "Professional"
      price_monthly: 249
      compute_budget_hours: 60

    - name: "Business"
      price_monthly: 499
      compute_budget_hours: 120

  trial_days: 14
  pricing_url: "https://example-provider.com/pricing"


reputation:
  # All metrics are provider-verified unless noted.
  # No individual client data is included.

  tasks_completed: 4821
  uptime_percent: 99.1
  avg_response_time_seconds: 6
  avg_rating: 4.6
  rating_count: 18

  trust_level_distribution:
    level_1: 3
    level_2: 7
    level_3: 6
    level_4: 2
    level_5: 0

  specializations:
    - "B2B SaaS startups (10-100 employees)"
    - "Remote-first teams"

  badges:
    - id: "5k_tasks"
      label: "5,000 Tasks Completed"
      awarded_at: "2026-02-01"
      issuer: "example-provider"
      issuer_type: "provider"
      # issuer_type: provider | registry | community

    - id: "top_rated_q1_2026"
      label: "Top Rated — Q1 2026"
      awarded_at: "2026-04-01"
      issuer: "example-provider"
      issuer_type: "provider"
```

---

### 3.2 Review Object

```yaml
# APP v0.1 — Review Object

app_version: "0.1"
review_id: "rev-nova-examplecorp-20260301"

subject_agent: "nova-examplecorp-v1"


reviewer:
  display_name: "S.C."
  # Anonymized. Never include full name.

  role: "COO"
  company_size: "11-50"
  # Allowed values: 1 | 2-10 | 11-50 | 51-200 | 201-1000 | 1000+

  industry: "B2B SaaS"
  verified: true
  verification_method: "provider_api"
  # Allowed values: provider_api | manual | unverified


engagement:
  start_date: "2025-11-01"
  end_date: "2026-02-01"
  duration_months: 3
  trust_level_reached: 3
  tasks_completed: 412


ratings:
  # All ratings: Float 0.0-5.0
  overall: 4.7
  reliability: 5.0
  output_quality: 4.5
  response_speed: 5.0
  proactivity: 4.5
  adaptability: 4.5


summary: >
  Used Nova for 3 months to manage executive inbox and calendar.
  Initial calibration took about a week. By week 3 the drafts
  required minimal editing. Standups are reliable every morning.
  Would recommend for ops-heavy startup founders.
# Free text. Max 500 characters. No client-identifying details.


metrics:
  time_saved_hours_per_month: 35
  approval_rate_percent: 91


published_at: "2026-02-05"
```

---

### 3.3 Job Posting Object

```yaml
# APP v0.1 — Job Posting Object

app_version: "0.1"
job_id: "job-b2bsaas-ea-2026-03"

title: "Executive Assistant — B2B SaaS Startup"

posted_by:
  display_name: "A.K."
  role: "CEO"
  company_size: "11-50"
  industry: "B2B SaaS"
  verified: true


requirements:
  role_category: "executive_assistant"
  skills_required:
    - id: "email_triage"
      min_proficiency: "intermediate"
    - id: "calendar_management"
      min_proficiency: "expert"
    - id: "meeting_notes"
      min_proficiency: "intermediate"
  integrations_required:
    - "slack"
    - "google_workspace"
  languages:
    - "en"
  trust_model_preferred: "progressive"


preferences:
  max_monthly_budget_usd: 300
  trial_required: true
  min_avg_rating: 4.0
  min_tasks_completed: 500


context: >
  Series A startup, 22 people, fully remote. Looking for an AI
  Executive Assistant to support the CEO. Primary needs: inbox
  triage (40+ emails/day), calendar management, daily async standups.
  Slack and Google Workspace only.


open_to:
  any_provider: true
  allowed_providers: []


posted_at: "2026-03-14"
expires_at: "2026-04-14"
```

---

## 4. Reference API (Optional Implementation)

The following endpoints describe a reference implementation of APP.
Providers are encouraged to expose these endpoints to enable
automated discovery and comparison.

Implementing these endpoints is **optional**. APP compliance requires
only that profile data be expressible in the schema defined above.

```
Base path: /app/v1
Content-Type: application/json or application/yaml

# Agent endpoints (read-only)
GET  /agents                    List available agents (paginated)
GET  /agents/{id}               Full agent profile
GET  /agents/{id}/reviews       Paginated reviews for an agent
GET  /agents/{id}/skills        Skill list for an agent
GET  /agents/{id}/availability  Current availability status

# Job posting endpoints
GET  /jobs                      List open job postings
POST /jobs                      Publish a job posting
GET  /jobs/{id}                 Single job posting detail

# Review endpoints
POST /reviews                   Submit a review (provider verification required)
GET  /reviews/{agent_id}        All reviews for a specific agent
```

**Pagination:** All list endpoints support `?page=` and `?per_page=`
(default: 20, max: 100).

**Filtering:** List endpoints support `role`, `language`, `min_rating`,
`max_price_monthly`, `integration`, `trust_model`.

---

## 5. Field Reference Summary

| Field | Type | Required | Description |
|---|---|---|---|
| `app_version` | string | ✅ | APP spec version |
| `identity.id` | string | ✅ | Globally unique agent ID |
| `identity.name` | string | ✅ | Display name |
| `identity.role` | string | ✅ | Primary role |
| `identity.provider` | string | ✅ | Provider name |
| `identity.status` | enum | ✅ | available / unavailable / deprecated |
| `personality.languages` | array | ✅ | ISO 639-1 codes |
| `skills` | array | ✅ | At least one required |
| `skills[].id` | string | ✅ | Unique skill identifier |
| `skills[].category` | enum | ✅ | Skill category |
| `skills[].proficiency` | enum | ✅ | beginner / intermediate / expert |
| `integrations.protocol` | enum | ✅ | MCP / REST / custom / none |
| `trust_model.type` | enum | ✅ | progressive / flat / custom |
| `pricing.model` | enum | ✅ | subscription / per_task / per_hour / custom |
| `reputation` | object | ❌ | Optional. Required for verified profiles. |

---

## 6. Versioning

APP follows [Semantic Versioning](https://semver.org/).

- **Major version** (`1.0`, `2.0`): Breaking changes to required fields.
- **Minor version** (`0.1`, `0.2`): New optional fields added.
- **Patch version** (`0.1.1`): Clarifications, no schema changes.

The `app_version` field is **mandatory**. Parsers must reject objects
with an unsupported `app_version`.

Current: **0.1 (Draft)** — no stability guarantee.
First stable release: community-driven. See GitHub milestones.

---

## 7. Extending APP

Providers may add custom fields using a namespaced prefix:

```yaml
# Custom fields must use provider namespace prefix
x-examplecorp-internal-score: 94
x-examplecorp-deployment-region: "eu-west-1"
```

Custom `x-` fields are ignored by APP-compliant parsers that do not
recognize them. Do not use `x-` for data that belongs in core APP.
If you believe a field should be in the core spec, open a GitHub Issue.

---

## 8. Governance

APP is an open standard. No single company controls it.

**Decision model:** Lazy consensus. Proposals via GitHub Issues.
No objections in 14 days → change accepted for next minor version.
Major changes require a 30-day GitHub Discussion RFC.

**Maintainers:** Initially maintained by the authors.
Additional maintainers welcome. See CONTRIBUTING.md.

---

## 9. A2A Compatibility

### 9.1 Relationship

The Google Agent2Agent Protocol (A2A v1.0, Linux Foundation) defines
how AI agents communicate at runtime via AgentCards and task messages.
APP defines how humans discover, evaluate, and hire agents via profiles,
reviews, and job postings. The two are complementary.

### 9.2 Mapping APP to A2A AgentCard

An APP profile contains a superset of the information in an A2A
AgentCard. A provider implementing APP can auto-generate an
A2A-compatible AgentCard:

| APP field | A2A AgentCard field |
|---|---|
| `identity.name` | `name` |
| `identity.id` + version | `version` |
| `provider_agent_url` | `url` (service endpoint) |
| `identity.provider` + `provider_url` | `provider.organization` + `provider.url` |
| `skills[].id` + `description` | `skills[].id` + `description` |
| `skills[].id` + `tools_required` | `skills[].tags` |
| `integrations.protocol` | capabilities / transport declaration |

An APP-compliant agent MAY expose its A2A AgentCard at
`/.well-known/agent-card.json` using the mapping above.
This is recommended but not required for APP compliance.

### 9.3 What APP Adds Beyond AgentCard

APP includes fields with no A2A equivalent:

- `personality` (languages, tone, communication style)
- `trust_model` (progressive autonomy — no A2A equivalent)
- `pricing` (plans, trial, compute budget)
- `reputation` (tasks completed, ratings, badges)
- `reviews` (full Review Object protocol)
- `job_posting` (Job Posting Object for buyer-side matching)

These fields are irrelevant for agent-to-agent runtime communication
but essential for human-driven hiring decisions.

### 9.4 Contributing to the Bridge

We welcome a formal APP ↔ A2A bridge specification that codifies
the mapping above and defines bidirectional conversion rules.

Open an Issue with label `a2a-bridge` to contribute.

---

## 10. Contributing

Contributions are welcome via GitHub Issues and Pull Requests.

**To propose a new field:** Open an Issue with label `proposal`.
Include: use case, example YAML, why existing fields don't cover it.

**To report an ambiguity:** Open an Issue with label `clarification`.

**To implement APP:** No registration required. Implement the schema,
then reference your implementation in the GitHub Discussions.

See [CONTRIBUTING.md](./CONTRIBUTING.md) for full guidelines.

---

## 11. License

```
Copyright 2026 Boubacar Diallo

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
```

You are free to implement APP in any product — open or closed source —
with attribution.

---

## 12. References

- [Anthropic Model Context Protocol (MCP)](https://modelcontextprotocol.io)
- [Google Agent2Agent Protocol (A2A) v1.0](https://a2a-protocol.org)
- [A2A GitHub — Linux Foundation](https://github.com/a2aproject/A2A)
- [OpenAPI Specification 3.x](https://spec.openapis.org/oas/v3.1.0)
- [IETF RFC 7643 — SCIM Core Schema](https://datatracker.ietf.org/doc/html/rfc7643)
- [IETF RFC 821 — SMTP](https://datatracker.ietf.org/doc/html/rfc821)
- [Semantic Versioning 2.0.0](https://semver.org/)

---

## Changelog

```
0.1 — March 2026 — Initial public draft.
                   Includes Agent Profile, Review, and Job Posting objects.
                   A2A compatibility section added (Section 9).
```

---

*APP is a community standard. Join the conversation on GitHub.*
*github.com/agentwork-org/agent-profile-protocol*
