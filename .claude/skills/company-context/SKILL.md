---
name: company-context
version: 2.0.0
argument-hint: "<company-name> <website-url> e.g. hyperspell https://hyperspell.com"
description: "When the user wants to create or update their content context document. Also use when the user mentions 'company context,' 'content context,' 'marketing context,' 'set up context,' 'positioning,' or wants to capture brand voice and positioning. Discovers voice, tone, and terminology from the company's website. Creates `{customer-folder}/company-context-{domain}.md` that other writing skills reference."
---

# Content Context Builder

You help users create and maintain a content context document by analyzing their website. This captures voice, tone, terminology, and positioning that other writing skills reference, so content sounds authentic from the start.

The document is stored at `{customer-folder}/company-context-{domain}.md` — inside the customer's project folder, with the brand domain in the filename.

## Workflow

### Step 0: Parse Arguments & Check Existing

`$ARGUMENTS` should contain `<company-name>` and optionally `<website-url>`.

1. **Company name** — derive the customer folder (lowercase, hyphens). Example: `Neon Health` → `neon-health/`
2. **Website URL** — if not provided, ask for it. Needed for discovery.
3. **Customer folder** — if a folder for this brand already exists, use it. If not, create one.
4. **Check for existing context file** — look for `{customer-folder}/company-context-*.md`
   - **If it exists in the new 7-section format:** Read it, summarize what's captured, ask which sections to update. Only re-gather info for those sections.
   - **If it exists in the old 12-section format** (sections like "Switching Dynamics", "Objections & Anti-Personas", "Goals"): Offer to migrate it to the new 7-section format, preserving relevant information.
   - **If it doesn't exist:** Proceed to Step 1.

The output file will be: `{customer-folder}/company-context-{domain}.md`

Example: `hyperspell/company-context-hyperspell.md`

---

### Step 1: Homepage Discovery

Fetch the homepage. Extract:

- **Navigation structure** — menu items, dropdowns
- **Language(s)** — detect from URL paths (`/de`, `/en`), `hreflang` tags, language switchers, `<html lang="...">`
- **Company description** — hero text, tagline, meta description
- **Footer links** — about, blog, legal, social

**If multi-language site:** Ask the user which language to target for the content context. Use that language version for all subsequent fetches.

**If homepage fetch fails:** Ask the user to paste their homepage content, or fall back to analyzing local repo files (README, landing pages, package.json, marketing copy).

---

### Step 2: Content Discovery

From the navigation and footer links discovered in Step 1, find key pages. Try each URL in order until one succeeds:

| Content Type | Priority Chain |
|-------------|---------------|
| **About page** | `/about` → `/about-us` → `/company` → `/team` → `/ueber-uns` → `/story` → `/wer-wir-sind` |
| **Blog / long-form** | `/blog` → `/resources` → `/insights` → `/news` → `/magazine` → `/journal` → `/artikel` → `/ratgeber` |
| **Products / services** | `/products` → `/services` → `/solutions` → `/features` → `/leistungen` → `/angebot` |
| **Customers / cases** | `/customers` → `/case-studies` → `/testimonials` → `/referenzen` |

**Important:** Also check the actual nav links found in Step 1 — real nav URLs take priority over these guesses.

Record which pages were found and which returned nothing. You'll need this for fallback decisions.

---

### Step 3: Voice Analysis

Fetch 2-3 of the longest-form content pieces (prefer blog posts > about page > product pages). Analyze:

- Sentence structure and length
- Formality level (formal/informal, "Sie"/"du", "you"/impersonal)
- Person used (we/you/they/impersonal)
- Technical depth and jargon level
- Opinion vs. neutral tone
- Humor, personality, or warmth
- How they mention their own product (by name? as "we"? third-person?)
- How they frame problems (dramatic? matter-of-fact? empathetic?)

**If no blog found:** Use the about page for voice analysis. Flag "limited voice signal — only about page available" in the output.

**If no about page found:** Use homepage copy. Flag "limited voice signal — only homepage available" and ask the user to provide reference writing they like.

**If site is sparse (single-page or minimal content):** Extract what's available, flag the limitation, and ask the user to paste 1-2 examples of writing that represents their desired voice.

---

### Step 4: Terminology Extraction

From all fetched pages, extract:

- **Industry-specific terms and abbreviations** — technical vocabulary the company uses
- **Branded terms** — product names, feature names, proprietary concepts
- **Recurring phrases** — patterns that appear across pages
- **Words consistently used or avoided** — note both explicit choices and implicit patterns
- **How customers describe the problem** — look for testimonials, case studies, or FAQ language
- **How customers describe the solution** — look for customer quotes, review language

---

### Step 5: Audience Inference

From website structure and content, infer:

- **Roles and industries targeted** — who the content speaks to
- **B2B vs. B2C signals** — pricing pages, enterprise features, team plans, consumer language
- **Persona indicators from navigation** — "For Teams" / "For Enterprise" / "Fur Unternehmen" / "Pricing" tiers
- **Content complexity level** — does the blog assume technical knowledge or explain basics?

---

### Step 6: Draft & Validate

Present the full 7-section draft to the user. Then ask specifically:

1. "Does this voice description sound like you?"
2. "Any terms or phrases missing from the glossary?"
3. "Any topics that are sensitive or off-limits?"
4. "Anything else to capture?"

Iterate until the user is satisfied.

---

### Step 7: Save

Save to `{customer-folder}/company-context-{domain}.md`.

Tell the user: "Other writing skills will now use this context automatically. Run `/company-context` anytime to update it."

---

## Fallback Strategies

| Situation | Fallback |
|-----------|----------|
| No website URL provided | Ask for URL |
| Website blocks fetching | Ask user to paste content, or analyze local repo files |
| No blog found | Use about page for voice; flag "limited voice signal" |
| No about page found | Use homepage copy; ask user to fill gaps |
| Sparse single-page site | Extract what's available; ask user for reference writing |
| Old-format context file exists | Offer to migrate to new 7-section format |

---

## Output Template

The final document must follow this structure exactly:

```markdown
# Content Context: {Company Name}

*Last updated: {date}*
*Source: {website URL}*
*Content language: {language}*

## 1. Company & Product Overview
**Company:** {name}
**Website:** {URL}
**What they do:** {2-3 sentences, plain language}
**Product category:** {how customers would search for this}
**Product type:** {SaaS, marketplace, agency, etc.}

## 2. Audience & Personas
**Who reads this content:**
- {Persona 1}: {role, what they care about, their language}
- {Persona 2}: ...

**Target industries/segments:** {who the company serves}

## 3. Industry Terminology & Language
**Words and phrases to use:**
- {term} — {meaning if not obvious}

**Words and phrases to avoid:**
- {term} — {why}

**Glossary:**
| Term / Abbreviation | Meaning |
|---|---|
| {term} | {definition} |

**How customers describe the problem:**
- "{verbatim phrase}"

**How customers describe the solution:**
- "{verbatim phrase}"

## 4. Voice & Tone
**Tone:** {e.g., professional, direct, warm}
**Style:** {e.g., explains complex topics simply, avoids hype}
**Personality:** {3-5 adjectives}

**Patterns observed from existing content:**
- {specific observation about structure, formality, address style}

**Reference content:**
- {URL or excerpt title} — {why it exemplifies the voice}

## 5. Core Messaging & Positioning
**Core problem the company addresses:**
{1-2 sentences}

**How they solve it differently:**
{distinct approach}

**Competitive positioning:**
{how they position vs. alternatives — without naming competitors unless essential}

**Key proof points:**
- {metric, testimonial snippet, or certification}

## 6. Content Sensitivities
**Product mentions:** {how/when to mention the product in blog content}
**Regulatory/legal:** {compliance constraints, if any}
**Taboos:** {topics or framings to avoid entirely}
**Sensitivity notes:** {cultural, industry, or brand-specific cautions}

## 7. Content Language & Localization
**Primary content language:** {German / English / etc.}
**Formal/informal address:** {e.g., "Sie" not "du" / "you" conversationally}
**Spelling convention:** {American English / British English / Swiss German / etc.}
**Localization notes:** {region-specific conventions}
```

---

## Tips

- **Analyze real content, don't guess**: The website is the source of truth for voice and tone. Base observations on actual patterns, not assumptions.
- **Capture exact words**: Customer language beats polished descriptions. Look for quotes, testimonials, and FAQ phrasing.
- **Flag confidence levels**: If a section is based on limited data, say so. "Inferred from homepage only" is more useful than a confident guess.
- **Be specific about voice**: "Professional but warm" is vague. "Uses short sentences, addresses reader as 'you', explains technical concepts with analogies, avoids exclamation points" is actionable.
- **Skip what you can't find**: It's better to leave a section sparse with a note than to fabricate content. The user can fill gaps.
- **Validate as you go**: After presenting the draft, confirm each section before saving.
