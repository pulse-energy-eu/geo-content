# Skills Overview

This document maps all available skills to a content production pipeline. All skills live in `.claude/skills/` and are invoked as slash commands (e.g. `/contentwriting`, `/copy-editing`).

## Pipeline Sequence

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  PHASE 1: CONTEXT                                                           │
│  ┌──────────────────────────┐                                               │
│  │ product-marketing-context │ → Brand voice, personas, positioning         │
│  └──────────────────────────┘                                               │
│  ┌──────────────────────────┐                                               │
│  │ seo-audit                │ → Technical SEO baseline (optional)           │
│  └──────────────────────────┘                                               │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  PHASE 2: WRITE                                                             │
│  ┌──────────────────────────┐                                               │
│  │ contentwriting           │ → Blog posts from briefings                   │
│  └──────────────────────────┘                                               │
│  ┌──────────────────────────┐                                               │
│  │ copywriting              │ → Landing pages, product pages                │
│  └──────────────────────────┘                                               │
│  ┌──────────────────────────┐                                               │
│  │ programmatic-seo         │ → Pages at scale (templates, directories)     │
│  └──────────────────────────┘                                               │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  PHASE 3: REFINE                                                            │
│  ┌──────────────────────────┐                                               │
│  │ copy-editing             │ → 7-sweep quality pass (clarity, proof, etc.) │
│  └──────────────────────────┘                                               │
│  ┌──────────────────────────┐                                               │
│  │ humanize                 │ → Remove AI patterns, add personality         │
│  └──────────────────────────┘                                               │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  PHASE 4: PUBLISH                                                           │
│  ┌──────────────────────────┐                                               │
│  │ framer                   │ → Convert to Framer CMS format                │
│  └──────────────────────────┘                                               │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Skills Summary Table

| Skill | Phase | Purpose | Trigger Phrases |
|-------|-------|---------|-----------------|
| **product-marketing-context** | 1. Context | Create/update brand positioning doc with voice, personas, competitors, customer language | "product context", "marketing context", "set up context", "positioning" |
| **seo-audit** | 1. Context | Technical SEO review (crawlability, indexation, Core Web Vitals, on-page) | "SEO audit", "technical SEO", "why am I not ranking", "SEO issues" |
| **contentwriting** | 2. Write | Blog posts from content briefings with SEO/GEO optimization | "write blog post", "create article", "blog from briefing" |
| **copywriting** | 2. Write | Landing pages, homepages, pricing pages, feature pages | "write copy for", "improve this copy", "marketing copy", "headline help" |
| **programmatic-seo** | 2. Write | Template-based pages at scale (locations, comparisons, integrations) | "programmatic SEO", "template pages", "pages at scale", "[X] vs [Y] pages" |
| **copy-editing** | 3. Refine | 7-sweep systematic editing (clarity, voice, proof, specificity, emotion, risk) | "edit this copy", "review my copy", "proofread", "polish this" |
| **humanize** | 3. Refine | Remove AI giveaways, add natural tone, personality, opinions | "humanize this", "make it natural", "remove AI giveaways", "tone check" |
| **framer** | 4. Publish | Convert markdown to Framer CMS-ready format | "format for Framer", "Framer format", "prepare for publishing" |
| **find-skills** | Utility | Discover and install new skills from skills.sh ecosystem | "find a skill", "how do I do X", "is there a skill for" |

---

## Typical Workflows

### Blog Post from Briefing

```
1. /product-marketing-context hyperspell
2. /contentwriting hyperspell/Hyperspell - 2.md
3. /copy-editing hyperspell/articles/{slug}-draft.md
4. /humanize hyperspell/articles/{slug}-edited.md
5. /framer hyperspell/articles/{slug}-final.md
```

### Landing Page

```
1. /product-marketing-context hyperspell
2. /copywriting hyperspell
3. /copy-editing hyperspell/articles/{slug}-draft.md
4. /framer hyperspell/articles/{slug}-final.md  (optional)
```

### SEO Content at Scale

```
1. /product-marketing-context hyperspell
2. /seo-audit hyperspell
3. /programmatic-seo hyperspell
4. /contentwriting hyperspell/Briefing - 1.md
```

---

## Output Naming Convention

Each pipeline step produces a file with a standardized suffix:

| Pipeline Step | Suffix | Example |
|---------------|--------|---------|
| Content briefing | (none, user-provided) | `hyperspell/Hyperspell - 2.md` |
| contentwriting | `-draft.md` | `hyperspell/articles/what-is-ai-agent-memory-draft.md` |
| copy-editing | `-edited.md` | `hyperspell/articles/what-is-ai-agent-memory-edited.md` |
| humanize | `-final.md` | `hyperspell/articles/what-is-ai-agent-memory-final.md` |
| framer | `-formatted.txt` | `hyperspell/articles/what-is-ai-agent-memory-formatted.txt` |

Slug is derived from the article's H1: lowercase, special chars removed, spaces → hyphens, max 60 chars.

---

## Customer Folder Structure

```
{customer}/
├── product-marketing-context-{domain}.md
├── Briefing - 1.md  (manually placed)
├── Briefing - 2.md
└── articles/
    ├── {slug}-draft.md
    ├── {slug}-edited.md
    ├── {slug}-final.md
    └── {slug}-formatted.txt
```

---

## Overlap Analysis

### No Direct Overlaps

Each skill has a distinct purpose:

| Skill A | Skill B | Distinction |
|---------|---------|-------------|
| contentwriting | copywriting | **Content** = educational blog posts. **Copy** = persuasive marketing pages. |
| copy-editing | humanize | **Editing** = marketing quality (proof, clarity, persuasion). **Humanize** = tone/AI detection. |
| seo-audit | programmatic-seo | **Audit** = diagnose issues. **Programmatic** = build pages at scale. |

### Complementary Pairs

These skills work together in sequence:

- **product-marketing-context** → feeds into all writing skills
- **contentwriting** → then **copy-editing** → then **humanize** → then **framer**
- **copywriting** → then **copy-editing** → then **framer**

---

## Cross-Reference Matrix

Which skills reference which:

| Skill | References | Referenced By |
|-------|-----------|---------------|
| **product-marketing-context** | — | contentwriting, copywriting, copy-editing, seo-audit, programmatic-seo, humanize |
| **contentwriting** | copy-editing, copywriting, product-marketing-context, seo-audit, programmatic-seo | copywriting, copy-editing, seo-audit, programmatic-seo |
| **copywriting** | copy-editing, humanize, contentwriting | contentwriting, copy-editing |
| **copy-editing** | copywriting, contentwriting, humanize | contentwriting, copywriting, seo-audit, programmatic-seo |
| **humanize** | contentwriting, copywriting, copy-editing | copywriting, copy-editing |
| **seo-audit** | programmatic-seo, contentwriting, copy-editing | contentwriting |
| **programmatic-seo** | seo-audit, contentwriting, copy-editing | seo-audit |
| **framer** | contentwriting, humanize, copy-editing | — |
| **find-skills** | — | — |

---

## Missing Skills (Potential Gaps)

Based on the pipeline, these skills could be added later:

| Potential Skill | Purpose | Current Workaround |
|-----------------|---------|-------------------|
| **content-briefing** | Create content briefings from keyword research | Manual research |
| **image-generation** | Generate/describe featured images | Manual in contentwriting |
| **internal-linking** | Add internal links across articles | Manual |
| **schema-markup** | Add structured data to pages | Manual |
| **email-sequence** | Email copy (nurture, launch, etc.) | Manual |
| **webflow** | Convert markdown to Webflow CMS format | Use framer as template |
| **wordpress** | Convert markdown to WordPress format | Use framer as template |

---

## Context File Location

All skills that need brand/voice context check for:

```
{customer-folder}/product-marketing-context-{domain}.md
```

Example: `hyperspell/product-marketing-context-hyperspell.md`

This file should be created once per client project using the `product-marketing-context` skill. The domain/brand name is included in the filename, and the file lives inside the customer folder — not at root.

---

## Directory Structure

```
.claude/skills/
├── product-marketing-context/
│   └── SKILL.md
├── seo-audit/
│   └── SKILL.md
│   └── references/
├── contentwriting/
│   └── SKILL.md
├── copywriting/
│   └── SKILL.md
│   └── references/
├── programmatic-seo/
│   └── SKILL.md
│   └── references/
├── copy-editing/
│   └── SKILL.md
│   └── references/
├── humanize/
│   └── SKILL.md
├── framer/
│   └── SKILL.md
└── find-skills/
    └── SKILL.md
```
