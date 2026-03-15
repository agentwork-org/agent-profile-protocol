# Why We Need APP

*A manifesto for the open agentic economy.*

---

## The internet standardized everything. Except agents.

Email works between Gmail and Outlook because of SMTP.
Websites render in any browser because of HTML.
APIs are documented in any language because of OpenAPI.
Package dependencies resolve across ecosystems because of npm and PyPI.

Every time the internet faced a fragmentation problem at scale, it solved it
with an open standard. A simple, agreed-upon format that anyone could implement,
no permission required.

We are at that moment again. And so far, nobody has acted.

---

## What is happening right now

In 2026, hundreds of companies are building AI agents — autonomous digital
workers that manage inboxes, schedule meetings, research leads, write code,
and handle customer support. The market is real, the technology works, and
adoption is accelerating.

But the ecosystem is completely fragmented.

Every platform describes its agents in its own format. Skills, trust levels,
pricing, reputation — all proprietary. If you hire an AI Executive Assistant
from Provider A and want to switch to Provider B, you start from zero. Your
agent's track record disappears. Its skills are described in a format nobody
else can read. Its reviews stay locked on the original platform.

There is no LinkedIn for AI agents. No Trustpilot. No Upwork.
No standard employment contract. No common vocabulary.

The industry is building in isolation, and buyers are paying the price.

---

## The protocols that exist solve different problems

The agentic ecosystem already has two important open standards:

**MCP** (Anthropic, 2024) — defines how an agent accesses tools and data.
It solves the agent-to-tool communication problem.

**A2A** (Google / Linux Foundation, 2025) — defines how agents communicate
with each other at runtime. It solves the agent-to-agent communication problem.

Both are excellent. Neither solves the problem we are describing.

When a business wants to hire an AI agent, they need to know:
- What can this agent do, specifically?
- Is it reliable? What is its track record?
- How autonomous is it? Can I start supervised and increase trust over time?
- What does it cost? Is there a trial?
- What do other companies say about it?
- Can I take my reviews with me if I switch platforms?

MCP and A2A do not answer these questions. They were not designed to.

APP fills that gap.

---

## What APP is

The **Agent Profile Protocol** is a simple, open, machine-readable format
for describing AI agents — not how they run, but who they are.

It describes:

- **Identity** — name, role, provider, version, status
- **Skills** — what the agent can do, at what proficiency, with which tools
- **Trust model** — how autonomy is structured (supervised → autonomous)
- **Pricing** — plans, compute budgets, trial terms
- **Reputation** — tasks completed, ratings, badges — all provider-verified
- **Reviews** — structured, verified client feedback, portable across platforms
- **Job postings** — a standard format for what businesses need

APP is not a marketplace. It is not a registry you must join.
It is a schema — a shared vocabulary — that anyone can implement,
in any product, for free, forever.

---

## Why open, why now

Two forces are converging that make this urgent.

**First**, major platforms are beginning to build proprietary agent registries.
What they build closed, the industry will be locked into. We have seen this
before with social networks, with app stores, with cloud platforms.
The window to establish an open standard is measured in months, not years.

**Second**, the developer community already knows this problem.
Ask any builder of AI agents whether their clients can compare them to
competitors on neutral ground. The answer is always no. Ask whether their
agents' reputations are portable. No. Ask whether there is a standard
employment contract for AI agents. No.

The problem is universally felt. The solution does not require permission
from any platform. It just requires someone to write the spec and publish it.

That is what APP is.

---

## What we are asking

APP v0.1 is a draft. It will improve with community input.

If you build AI agents, we want to know: does this schema describe your
agents accurately? What is missing? What is wrong?

If you buy or evaluate AI agents, we want to know: what information do you
need to make a hiring decision that APP does not yet capture?

If you work on MCP or A2A or any other agentic protocol, we want to know:
how can APP complement what you are building?

The spec is at [SPEC.md](../SPEC.md). The schemas are in [/schemas](../schemas/).
Open an Issue. Start a Discussion. Submit a PR.

An open agentic economy is better for everyone.
Let's build the infrastructure it needs.

---

*Published March 2026.*
*github.com/agentwork-org/agent-profile-protocol*
