# geo-content

AI-powered content production pipeline using Claude Code slash commands. Write, edit, humanize, and format blog posts and marketing copy with consistent brand voice.

## Quick Start: New Company

### 1. Setup (once per customer)

```
/product-marketing-context acme
```

Creates `acme/product-marketing-context-acme.md` with brand voice, personas, positioning, and customer language. Every other command reads this automatically.

### 2. Blog post pipeline

Run these in order, passing the output of each step to the next:

```
/contentwriting acme/Briefing - 1.md          → acme/articles/{slug}-draft.md
/copy-editing acme/articles/{slug}-draft.md    → acme/articles/{slug}-edited.md
/humanize acme/articles/{slug}-edited.md       → acme/articles/{slug}-final.md
/framer acme/articles/{slug}-final.md          → acme/articles/{slug}-formatted.txt
```

Each command tells you the next one to run.

### 3. Other commands

| Command | What it does |
|---------|-------------|
| `/copywriting acme` | Landing page / marketing page copy |
| `/seo-audit acme` | Technical SEO review |
| `/programmatic-seo acme` | SEO pages at scale (templates, directories) |
| `/find-skills` | Discover and install new skills from skills.sh |

## Folder structure after a full run

```
acme/
├── product-marketing-context-acme.md
├── Briefing - 1.md          ← you place this here
└── articles/
    ├── {slug}-draft.md
    ├── {slug}-edited.md
    ├── {slug}-final.md
    └── {slug}-formatted.txt
```

## Where commands live

All slash commands are in `.claude/skills/<name>/SKILL.md`. Type `/` in Claude Code to see them in autocomplete. See [SKILLS.md](SKILLS.md) for the full pipeline diagram, cross-reference matrix, and detailed documentation.
