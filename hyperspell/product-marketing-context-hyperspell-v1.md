# Product Marketing Context

*Last updated: 2026-02-04*

## Product Overview
**One-liner:** The memory and context layer for AI agents.

**What it does:** Hyperspell gives AI agents automatic memory by connecting them to users' data sources (email, Slack, docs, CRM, calendar). It continuously indexes documents into a knowledge graph and retrieves relevant context for each agent interaction—eliminating months of engineering work for data pipelines, OAuth, and retrieval tuning.

**Product category:** AI agent memory infrastructure / Context engineering platform

**Product type:** B2B SaaS (managed platform)

**Business model:** Usage-based pricing (private beta, invite-based access). Enterprise-focused with SOC 2 and GDPR compliance.

## Target Audience
**Target companies:** AI product companies, technical startups, and enterprises building agentic applications that need contextual awareness. Companies building AI assistants, copilots, sales agents, support agents, or research tools.

**Decision-makers:** Technical founders, CTOs, senior engineers, and AI/ML team leads evaluating memory infrastructure.

**Primary use case:** Connecting AI agents to end-user workspace data so agents can understand the user's world—their people, projects, communications, and documents.

**Jobs to be done:**
- Give my AI agent access to user data without building OAuth integrations from scratch
- Make my agent remember context across sessions without building a custom memory layer
- Retrieve relevant context for agent queries without months of RAG tuning

**Use cases:**
- Customer support agent that knows the customer's history and account details
- Sales agent that understands deals, contacts, and past communications
- Personal assistant that knows the user's calendar, team, and priorities
- Research agent that can search across a user's documents and notes

## Personas
| Persona | Cares about | Challenge | Value we promise |
|---------|-------------|-----------|------------------|
| Technical Founder | Speed to market, not reinventing infrastructure | Building OAuth, data pipelines, and retrieval from scratch takes months | Go from concept to paid pilots in 48 hours |
| AI Engineer | Retrieval quality, developer experience | RAG tuning is tedious; memory systems are complex to build | One-line integration, continuous improvement |
| CTO/Technical Lead | Scalability, security, maintainability | Evaluating build vs. buy, worried about vendor lock-in | SOC 2 + GDPR compliant, 40+ integrations maintained for you |

## Problems & Pain Points
**Core problem:** AI agents are stateless by default. They're "clueless geniuses"—powerful reasoners with no knowledge of the user's world. Every session starts from zero.

**Why alternatives fall short:**
- Building from scratch requires 3-6 months of OAuth integrations, ETL pipelines, embedding optimization, and retrieval tuning
- Conversation-only memory (Mem0-style) doesn't capture the user's existing data—only what they say in conversation
- Generic RAG solutions don't handle multi-source workspace data or keep indexes fresh

**What it costs them:** 3-6 months of engineering time. Slower onboarding of new customers. Agents that ask the same questions repeatedly and fail at personalization.

**Emotional tension:** Frustration from rebuilding the same data infrastructure every AI startup needs. Anxiety about delivering a product that feels "dumb" compared to competitors with better context.

## Competitive Landscape
**Direct:**
- Mem0 — Open-source memory layer focused on conversation-derived memory. Falls short on external data source integration; primarily extracts memories from conversations rather than indexing existing user data.
- Zep — Managed knowledge graph for enterprise assistants. Falls short on breadth of integrations (SDK-level vs. pre-built OAuth).
- Supermemory — Universal memory API. Falls short on depth and maturity (newer entrant).

**Secondary:**
- Custom RAG pipelines (LangChain + Pinecone) — Requires building everything yourself; no pre-built integrations.
- Letta/MemGPT — Agent framework with self-managed memory. Falls short for teams that want a memory API, not a full agent framework.

**Indirect:**
- Doing nothing / conversation buffers only — Falls short when users expect agents to remember and understand their world.

## Differentiation
**Key differentiators:**
- 40+ pre-built integrations with OAuth handling (Gmail, Slack, Notion, Salesforce, HubSpot, etc.)
- Dual search modes: indexed search for speed, live search for privacy-sensitive use cases
- Knowledge graph retrieval—not just vector search
- Pre-built Hyperspell Connect components for user onboarding
- SOC 2 + GDPR compliance out of the box

**How we do it differently:** We connect to the user's actual workspace tools and continuously index their data, rather than only learning from conversations. We handle all the OAuth, data normalization, and freshness automatically.

**Why that's better:** Agents understand the user's full context—not just what they've said in conversation. Teams ship in days, not months.

**Why customers choose us:**
- "Saved us the heavy lift" of building custom infrastructure
- "Onboard new customers five times faster"
- "Went from concept to paid pilots in 48 hours"

## Objections
| Objection | Response |
|-----------|----------|
| "We can build this ourselves" | You can—and it'll take 3-6 months of senior engineering time. We've done this already, with 40+ integrations maintained and optimized. Focus your team on your core product. |
| "What about data privacy?" | We're SOC 2 certified and GDPR compliant. We offer live search mode for privacy-sensitive use cases where data never leaves the source. Your users control what they share. |
| "You're a startup—will you be around?" | We're backed by [investors if disclosed], serving companies like Hobbes, Scale Agentic, and Entelligence in production. |

**Anti-persona:** Teams that want full control over their infrastructure and have 6+ months to invest in building memory from scratch. Hobby projects or very early prototypes that don't need persistent memory yet.

## Switching Dynamics
**Push:** Frustration with rebuilding OAuth integrations. Wasted months on RAG tuning that still underperforms. Agents that feel "dumb" and ask repetitive questions.

**Pull:** Ship in days instead of months. 40+ integrations ready to go. Agents that actually understand the user's world.

**Habit:** Teams already invested in custom pipelines. Familiarity with LangChain/LlamaIndex patterns. "We've always built this ourselves."

**Anxiety:** Vendor lock-in. Trusting a startup with user data. Learning a new API.

## Customer Language
**How they describe the problem:**
- "My agent has no idea who the user is"
- "We're spending all our time on OAuth instead of building our product"
- "RAG feels like a dead end—retrieval quality is terrible"
- "Users hate repeating themselves to the agent"

**How they describe us:**
- "The memory layer so we don't have to build it"
- "Plug-and-play context for our agent"
- "Saved us months of infrastructure work"

**Words to use:** memory layer, context layer, workspace data, integrations, knowledge graph, retrieval, agent memory, persistent context

**Words to avoid:** AI brain, artificial memory, mind reading, learning your thoughts (avoid anthropomorphization)

**Glossary:**
| Term | Meaning |
|------|---------|
| Context engineering | The discipline of designing what information an AI agent receives, when, and how |
| Indexed search | Pre-indexed data for fast retrieval |
| Live search | Real-time queries to source APIs for freshest data / privacy-sensitive use cases |
| Hyperspell Connect | Pre-built UI components for users to connect their accounts |
| Memory layer | Infrastructure that gives agents persistent knowledge across sessions |

## Brand Voice
**Tone:** Technical, measured, confident without being boastful. Respects the audience's intelligence.

**Style:** Direct, clear, avoids hype. Explains complex concepts with precision. Uses concrete examples over abstract claims. Historical/technical framing preferred over speculation.

**Personality:** Thoughtful, practitioner-first, builder credibility, anti-hype

## Proof Points
**Metrics:**
- 40+ pre-built integrations
- SOC 2 + GDPR compliant
- "Five times faster" customer onboarding (customer claim)
- "48 hours from concept to paid pilots" (Scale Agentic)

**Customers:** Hobbes, Entelligence, Scale Agentic, Micro, Bear, Proxis, Virio, Aside, Eragon

**Testimonials:**
> "Saved us the heavy lift of developing this completely in-house—which could have taken months and still not delivered a complete solution." — Arjun Athreya, CTO at Hobbes

> "Integration was quick, and we can now onboard new customers five times faster." — Anish Chopra, Founder

> "Helped us go from concept to paid pilots in just 48 hours." — Anna Yuan, CEO at Scale Agentic

**Value themes:**
| Theme | Proof |
|-------|-------|
| Speed to market | "48 hours from concept to paid pilots" |
| Reduced engineering burden | "Saved us months" / "heavy lift" |
| Breadth of integrations | 40+ pre-built with OAuth |
| Enterprise-ready | SOC 2 + GDPR |

## Goals
**Business goal:** Become the default memory infrastructure for AI agent products.

**Conversion action:** Request early access / Start with sandbox

**Current metrics:** Private beta, invite-based access
