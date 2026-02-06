# geo-content

AI-powered content production pipeline using Claude Code slash commands. Write, refine, and format blog posts with consistent brand voice, SEO optimization, and structured data.

## Pipeline

### Once per customer

```
/company-context acme https://acme.com    → acme/company-context-acme.md
/schema acme                              → acme/page-schema.tsx + acme/layout-schema.tsx
```

### Per article

```
/contentwriting acme/Briefing - 1.md      → acme/output/{slug}-draft.md
/refine acme/output/{slug}-draft.md     → acme/output/{slug}-final.md
/image-pexels acme/output/{slug}-final.md  → downloads hero image
/framer acme/output/{slug}-final.md     → acme/output/{slug}-framer.md
```

## Commands

### Setup (once per customer)

| Command | What it does | Input | Output |
|---------|-------------|-------|--------|
| `/company-context` | Discovers voice, tone, terminology, and positioning from a company's website. | Company name + URL | `{customer}/company-context-{domain}.md` |
| `/schema` | Generates one-time Next.js JSON-LD templates that build structured data from article frontmatter. | Customer folder (+ optional sample article) | `{customer}/page-schema.tsx` + `{customer}/layout-schema.tsx` |

### Article pipeline (per post)

| Command | What it does | Input | Output |
|---------|-------------|-------|--------|
| `/contentwriting` | Writes a full blog post from a content briefing, with SEO/GEO optimization and complete YAML frontmatter (including `wordCount` + `faqs`). | Briefing `.md` file | `{customer}/output/{slug}-draft.md` |
| `/refine` | Verifies accuracy, checks voice consistency, removes AI generation artifacts, and validates frontmatter completeness. | `-draft.md` | `{customer}/output/{slug}-final.md` |
| `/image-pexels` | Finds and downloads a royalty-free hero image from Pexels matching the article topic. | `-final.md` | Downloaded image file |
| `/framer` | Converts a finished article to Framer CMS-ready format. | `-final.md` | `{customer}/output/{slug}-framer.md` |

### Other commands

| Command | What it does | Input | Output |
|---------|-------------|-------|--------|
| `/copywriting` | Writes or rewrites marketing copy for landing pages, homepages, pricing pages. | Customer folder + page URL or brief | Copy in markdown |
| `/copy-editing` | Edits existing marketing copy or blog content through multiple focused passes. | File to edit | Edited file |
| `/seo-audit` | Audits technical SEO issues on a live site. | Site URL | Audit report |
| `/programmatic-seo` | Creates SEO-driven pages at scale using templates and data. | Customer folder + strategy | Template files + data |
| `/find-skills` | Discovers and installs new skills from skills.sh. | Search query | Installed skill |

## Folder structure

```
acme/
├── company-context-acme.md       ← brand voice & positioning
├── page-schema.tsx               ← one-time Next.js JSON-LD (per-page)
├── layout-schema.tsx             ← one-time Next.js JSON-LD (global)
├── Briefing - 1.md               ← you place this here
└── output/
    ├── {slug}-draft.md
    └── {slug}-final.md
```

## Where commands live

All slash commands are in `.claude/skills/<name>/SKILL.md`. Type `/` in Claude Code to see them in autocomplete.
