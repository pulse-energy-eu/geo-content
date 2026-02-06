# Content Briefing: Integration Landing Pages (Programmatic SEO)

| Field | Value |
| :---- | :---- |
| **H1 template** | Connect \[Integration\] to Your AI Agent with Hyperspell |
| **Format** | Programmatic SEO landing pages (NOT blog posts) |
| **Playbook** | Integrations (Playbook \#8) |
| **Target queries** | "AI agent \[tool\] integration", "connect AI agent to \[tool\]", "give AI agent access to \[tool\]", "\[tool\] for AI agents", "AI agent \[tool\] context" |
| **Primary persona** | Developers building AI agents who need to connect to a specific tool (e.g., they're building a sales agent and searching for "AI agent Salesforce integration") |
| **Funnel stage** | Mid-to-bottom funnel (solution-aware, tool-specific) |
| **Word count target** | 800-1,200 words per page (lean, focused) |
| **Scale** | 15-20 pages initially (top integrations), expandable to 40+ |
| **GEO opportunity** | Integration-specific queries generate citations in the dataset: Zapier+Slack+Claude (67 answers, 1.93 avg), Zapier+Gmail+Claude (33 answers, 2.91 avg), n8n+Slack (30 answers, 1.47 avg), n8n+Gmail (21 answers, 2.38 avg), heyhelp.ai+Gmail (26 answers, 5.42 avg). These are tool-specific queries where AI models look for specific integration documentation. No memory platform has marketing pages for their integrations \- only docs. |
| **Competitor benchmark** | Zapier integration pages, n8n integration pages, heyhelp.ai how-to pages. These are workflow/automation tools, not memory platforms. Direct competitors (Mem0, Zep, Supermemory) do NOT have integration marketing pages. |
| **Why Hyperspell** | Hyperspell has 40+ integrations. Each is a keyword opportunity. The docs have technical setup pages, but no SEO-optimized marketing pages explaining the use case and value. |
| **Docs overlap** | docs.hyperspell.com has individual integration pages (e.g., /integrations/all/slack) but these are technical setup guides. These landing pages focus on the "why" and use case, not the "how to configure." |
| **URL structure** | `hyperspell.com/integrations/[tool-name]` (e.g., `/integrations/slack`, `/integrations/gmail`) |

---

## Page Template (applies to all integration pages)

### H1: Connect \[Integration\] to Your AI Agent with Hyperspell

### Section 1: The Problem (2-3 paragraphs)

- What \[Integration\] data is valuable for AI agents  
- Why agents need access to this data (specific use cases)  
- The challenge of building this connection yourself (OAuth, API pagination, data normalization, keeping it fresh)

### Section 2: How Hyperspell \+ \[Integration\] Works (2-3 paragraphs \+ diagram)

- What data Hyperspell indexes from \[Integration\]  
- How the connection is established (Hyperspell Connect, OAuth)  
- What queries your agent can answer with this data  
- Example: "With \[Integration\] connected, your agent can answer: \[2-3 example natural language queries\]"

### Section 3: Use Cases (3-4 bullet points)

- Specific, concrete use cases for this integration  
- Tailored to the tool's domain (CRM for Salesforce, communication for Slack, etc.)

### Section 4: Developer Experience (code snippet)

- 5-line code example showing how to query \[Integration\] data through Hyperspell's SDK  
- Emphasize simplicity

### CTA

- "Get started with Hyperspell" \-\> link to docs quickstart or signup

### Related Integrations

- Link to 3-4 related integration pages (creates internal linking mesh)

---

## Priority Integration Pages (Phase 1: Top 15\)

Based on search volume potential and the citation data:

| Priority | Integration | Target query examples | Why prioritize |
| :---- | :---- | :---- | :---- |
| 1 | **Slack** | "AI agent Slack integration", "connect AI agent to Slack" | Slack+AI queries generate 200+ answers in the dataset across multiple pages |
| 2 | **Gmail** | "AI agent Gmail integration", "give AI agent email access" | Gmail+AI queries generate 100+ answers. heyhelp.ai's Gmail page gets 5.42 avg citations |
| 3 | **Notion** | "AI agent Notion integration", "connect AI to Notion" | Most popular Hyperspell integration per docs |
| 4 | **Google Calendar** | "AI agent calendar integration" | Calendar queries appear in dataset (Claude calendar integration) |
| 5 | **Salesforce** | "AI agent Salesforce", "Salesforce AI integration" | Enterprise CRM \- high commercial intent |
| 6 | **HubSpot** | "AI agent HubSpot", "HubSpot AI integration" | CRM \+ high search volume |
| 7 | **Google Drive** | "AI agent Google Drive", "connect AI to Google Drive" | Document access is a core use case |
| 8 | **Jira** | "AI agent Jira integration" | Dev/PM tool with high search volume |
| 9 | **GitHub** | "AI agent GitHub integration" | Developer audience alignment |
| 10 | **Linear** | "AI agent Linear integration" | Startup/dev audience alignment |
| 11 | **Microsoft Outlook** | "AI agent Outlook integration" | Enterprise email alternative to Gmail |
| 12 | **Google Docs** | "AI agent Google Docs" | Document memory use case |
| 13 | **Zoom** | "AI agent Zoom integration" | Meeting context use case |
| 14 | **Discord** | "AI agent Discord integration" | Community/developer tool |
| 15 | **Airtable** | "AI agent Airtable integration" | Data/spreadsheet use case |

---

## Unique Content Requirements Per Page

To avoid thin content / doorway page penalties, each page MUST include:

1. **Integration-specific data types**: What specific data Hyperspell indexes from this tool (e.g., for Slack: messages, channels, threads, reactions, files; for Salesforce: contacts, opportunities, accounts, activities)  
2. **Integration-specific use cases**: Not generic "improve productivity" but specific scenarios (e.g., for Slack: "Your agent can find the conversation where the team decided to change the pricing model last quarter")  
3. **Integration-specific example queries**: 3-4 natural language questions the agent can answer with this data connected  
4. **Integration-specific code example**: Real SDK call with source-specific parameters

---

## GEO Optimization Notes

- Each page should include the integration name in H1, first paragraph, and at least 2 subheadings  
- Include structured data (SoftwareApplication schema with integration mentions)  
- The example queries section is highly citable by AI ("With Hyperspell \+ Slack, an agent can answer questions like...")  
- Create a hub page at `/integrations` that links to all individual pages (hub-and-spoke architecture)  
- Each page links to 3-4 related integration pages (internal linking mesh)  
- Include a "last updated" date on each page

## Internal Linking

- Hub page: `/integrations` links to all individual pages  
- Each page links to 3-4 related integrations  
- Each page links to docs.hyperspell.com integration setup guide  
- Each page links to quickstart guide  
- Blog posts link to relevant integration pages (e.g., "How to Build Persistent Memory" links to integration pages when discussing data sources)

