# GEO Briefing Checklist

Use this checklist when creating content briefings to ensure they include everything the `/contentwriting` skill needs to produce optimally citable articles with complete YAML frontmatter.

## Metadata Fields

These fields populate the YAML frontmatter in the article draft. Include them in the briefing's metadata table.

| Field | Required | Purpose | Example |
|-------|----------|---------|---------|
| **H1** | yes | Article title and `title` in YAML | "How to Evaluate Private Equity: A Complete Guide..." |
| **Target slug** | yes | URL path, used in YAML `slug` and by `/schema` for schema URLs | `/blog/evaluate-private-equity-guide` |
| **Primary keyword** | yes | YAML `seo.primaryKeyword`, drives H1/H2 placement | `evaluate private equity` |
| **Secondary keywords** | yes | YAML `seo.secondaryKeywords`, woven into body | `private equity performance metrics, IRR MOIC DPI` |
| **Search intent** | yes | YAML `seo.searchIntent`, shapes article structure and CTAs | `informational`, `commercial`, `comparison` |
| **Search volume** | recommended | YAML `seo.searchVolume`, useful for prioritization | `12,100/mo` |
| **Author** | recommended | YAML `author`, falls back to company-context default author | `Manu Ebert, Founder` |
| **Format** | recommended | YAML `format`, guides structure (fallback vs. outline) | `pillar_post`, `long_form_guide`, `comparison`, `how_to` |
| **Word count target** | recommended | Guides section depth | `2000-2500` or `3000-4000` |
| **Target persona** | recommended | Calibrates vocabulary and explanation depth | `Institutional investors, asset managers, LPs` |

## Reference Content with URLs

Every source listed in the briefing should include a working URL. These become hyperlinked inline citations in the article body and entries in the Sources section.

For each source, provide:
- **URL** (required)
- **What to learn from it** (recommended) -- tells the writer where to cite it

**Example:**
```
| URL | What to learn from it |
|-----|----------------------|
| https://www.wellington.com/en/insights/understanding-private-equity-performance | Three-metric framework (IRR/MOIC/DPI); J-curve explanation |
```

Sources without URLs will be flagged with `[VERIFY]` placeholders in the draft.

## GEO Structural Elements

These guide the article's structure for AI citability.

### Comparison Table Targets
Specify which concepts the article should compare in a structured table. Tables are 40% more likely to be cited by AI systems than prose.

**Example:** "Include a comparison table: context engineering vs prompt engineering vs RAG"

### Named Frameworks
If the article should introduce an original model or methodology, name it in the briefing. Named frameworks are among the highest-leverage GEO assets because they are uniquely citable.

**Example:** "Introduce 'The Four Layers of Agent Context' as the article's original framework"

### Target FAQ Questions
List 3-5 "People Also Ask" questions from PAA/SERP research. The contentwriting skill will turn these into a FAQ section. Pages with FAQ content are 3.2x more likely to appear in AI Overviews.

**Example:**
- "What is the difference between context engineering and prompt engineering?"
- "Is context engineering the same as RAG?"
- "Do I need context engineering if my model supports 1M+ tokens?"

### Entity Association Targets
Specify which topics should be explicitly linked to the company name. The contentwriting skill targets 5-7 company mentions in strategic positions, but the briefing should guide which concepts to associate with the brand.

**Example:** "Associate Hyperspell with: context layer, memory infrastructure, four-layer context model, agentic computing"

---

## Briefing Template

Add these sections to the bottom of each content briefing:

```markdown
## Metadata

| Field | Value |
|-------|-------|
| H1 | [Title] |
| Target slug | [/blog/your-article-slug] |
| Format | [pillar_post / long_form_guide / comparison / how_to / listicle] |
| Word count target | [e.g. 2000-2500] |
| Primary keyword | [keyword] |
| Search volume | [monthly volume if known] |
| Secondary keywords | [kw1, kw2, kw3] |
| Search intent | [informational / commercial / comparison] |
| Target persona | [Who this is for] |
| Author | [Name, Role] |

## Reference Content

| URL | What to learn from it |
|-----|----------------------|
| [url] | [What the writer should cite from this source] |

## GEO Elements

**Comparison table:** [What concepts to compare in table format]
**Named framework:** [Original framework name, if applicable]
**FAQ questions:**
- [Question 1]
- [Question 2]
- [Question 3]
**Entity associations:** [Topics to link to company name]
```
