# Skills Overview

This document maps all available skills to a content production pipeline.

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
│  │ cms-format               │ → Convert to Framer/WordPress/CMS format      │
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
| **cms-format** | 4. Publish | Convert markdown to CMS-ready format (Framer, WordPress, etc.) | "format for CMS", "Framer format", "prepare for publishing" |
| **find-skills** | Utility | Discover and install new skills from skills.sh ecosystem | "find a skill", "how do I do X", "is there a skill for" |

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
- **contentwriting** → then **humanize** → then **cms-format**
- **copywriting** → then **copy-editing** → then **cms-format**

---

## Typical Workflows

### Blog Post from Briefing

```
1. /product-marketing-context  (once per client)
2. /contentwriting             (briefing → article v1)
3. /humanize                   (article v1 → v2)
4. /cms-format                 (v2 → framer.txt)
```

### Landing Page

```
1. /product-marketing-context  (once per client)
2. /copywriting                (create page copy)
3. /copy-editing               (7-sweep polish)
4. /cms-format                 (optional)
```

### SEO Content at Scale

```
1. /product-marketing-context  (once per client)
2. /seo-audit                  (baseline assessment)
3. /programmatic-seo           (template strategy)
4. /contentwriting             (individual pages)
```

---

## Missing Skills (Potential Gaps)

Based on the pipeline, these skills could be added later:

| Potential Skill | Purpose | Current Workaround |
|-----------------|---------|-------------------|
| **content-briefing** | Create content briefings from keyword research | Manual research |
| **image-generation** | Generate/describe featured images | Manual in contentwriting |
| **internal-linking** | Add internal links across articles | Manual |
| **schema-markup** | Add structured data to pages | Referenced in seo-audit |
| **email-sequence** | Email copy (nurture, launch, etc.) | Referenced in copywriting |

---

## Directory Structure

```
skills/
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
├── cms-format/
│   └── SKILL.md
└── find-skills/
    └── SKILL.md
```
