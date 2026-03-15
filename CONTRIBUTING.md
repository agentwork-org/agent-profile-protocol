# Contributing to APP

Thank you for your interest in the Agent Profile Protocol.

APP is an open standard governed by its community.
No single company controls it. Your contributions shape its direction.

---

## Ways to Contribute

### 1. Report an ambiguity
If something in the spec is unclear, open an Issue with label `clarification`.
Include: the unclear section, your interpretation, what you expected.

### 2. Propose a new field
If you need a field APP doesn't have, open an Issue with label `proposal`.
Include:
- Use case: what problem does this field solve?
- Example YAML showing the proposed field
- Why existing fields don't cover it

### 3. Contribute to A2A compatibility
Want to formalize the APP ↔ A2A bridge specification?
Open an Issue with label `a2a-bridge`.
This includes bidirectional conversion rules and tooling proposals.

### 4. List your implementation
If you implement APP, open a Discussion in the `Implementations` category.
Include: platform name, link, APP version implemented, notes.

### 5. Fix documentation
Typos, grammar, broken links — PRs welcome, no Issue required.

---

## Decision Process

APP uses a **lazy consensus** model (borrowed from Apache Foundation):

- Proposals as GitHub Issues
- 14-day comment period for minor changes (new optional fields, clarifications)
- 30-day comment period for major changes (posted as GitHub Discussion)
- No substantive objections → change accepted for next minor version
- Breaking changes require explicit maintainer approval and a major version bump

---

## What We Will Not Accept

- Fields that encode platform-specific business logic
- Fields that identify individual clients or expose private data
- Changes that break backward compatibility without a major version bump
- Proprietary extensions disguised as core fields (use `x-yourprovider-` instead)

---

## Code of Conduct

Be direct. Be respectful. Focus on technical merit.
We are building infrastructure for the agentic economy.
Decisions should be made on that basis.

---

*APP — github.com/agentwork-org/agent-profile-protocol*
