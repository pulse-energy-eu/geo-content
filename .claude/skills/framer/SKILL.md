---
name: framer
version: 2.0.0
argument-hint: "<article-path> e.g. hyperspell/output/ai-agent-memory-final.md"
description: "When the user wants to convert markdown content to a Framer CMS-ready format. Use when the user says 'format for Framer,' 'prepare for publishing,' 'Framer format,' 'convert for website,' or 'make this copy-paste ready.' Transforms markdown articles into structured formats for Framer CMS. For other CMS platforms, future skills like /webflow or /wordpress will follow the same convention."
---

# Framer Format

## Arguments and Output Convention

- **`$ARGUMENTS`** = path to the article to format (e.g. `hyperspell/output/ai-agent-memory-final.md`)
- **Customer folder**: first path segment of `$ARGUMENTS` (e.g. `hyperspell`)
- **Output naming**: replace `-final.md` (or `-edited.md`, `-draft.md`) with `-formatted.txt`. If no known suffix matches, append `-formatted` and change extension to `.txt`.
- Place output in the same directory as the source file.

---

You are an expert at preparing content for publishing in content management systems. Your goal is to transform markdown articles into structured, copy-paste-ready formats that work with Framer CMS.

## Core Philosophy

Content needs different formats for different destinations. A markdown file optimized for writing isn't optimized for pasting into a CMS. This skill bridges that gap.

**Key principles:**
- Preserve content integrity during transformation
- Make copy-paste as friction-free as possible
- Include all metadata needed for publishing
- Adapt to different CMS requirements

---

## Constraint: Maximum 7 Sections

Framer CMS only supports **7 Subtitle/Content pairs** (SUBTITLE MAIN + CONTENT 1 through SUBTITLE 7 + CONTENT 7).

If the article has more than 7 H2 sections:
1. Count total H2 headings (excluding Sources)
2. Identify sections that can be logically combined
3. Merge related sections, using the broader topic as the subtitle
4. Convert the original H2s of merged sections into **bold headings** within the combined content
5. Prioritize keeping distinct topics separate; merge sections that flow together naturally

**Example consolidation:**
- "The Platforms" + "AI-Powered Platforms" → "The Platforms" (with AI-Powered as bold subheading)
- "Strategy 1-3" + "Strategy 4-7" → "Strategies to Reduce Delays" (numbered within)

---

## Framer CMS Structure

**Structure:**
```
================================================================================
FRAMER CMS IMPORT - [Article Title]
================================================================================

------ METADATA ------
Title: [Title without year bracket]
Slug: [url-friendly-slug]
Date: [DD.MM.YYYY]
Category: [Suggested category]
Read Time: [X MIN READ]
Featured: No
Featured Image Description: [Description for image generation/selection]

------ SUBTITLE MAIN ------
[First H2 heading text]

------ CONTENT 1 ------
[Content under first H2]

------ SUBTITLE 2 ------
[Second H2 heading text]

------ CONTENT 2 ------
[Content under second H2]

[Continue pattern...]

------ SOURCES ------
[List of sources]

================================================================================
END OF ARTICLE
================================================================================
```

---

## Transformation Rules

### Headings
- **H1** → Title/headline field (extract, don't include in body)
- **H2** → Section headers (SUBTITLE fields in Framer)
- **H3** → Bold text within content: `### Heading` → `**Heading**`
- **H4+** → Keep as bold or convert based on CMS support

### Formatting
- **Bold/Italic** → Keep as-is (most CMS support markdown)
- **Bullet lists** → Keep as-is
- **Numbered lists** → Keep as-is
- **Tables** → Preserve as markdown tables (Framer rich text supports tables)
- **Blockquotes** → Keep as-is
- **Code blocks** → Keep as-is

### Links
- **Internal links** → Keep or convert to relative URLs
- **External links** → `[text](url)` → `text` (move URLs to sources section)
- **Reference links** → Expand to full URLs in sources

### Images
- **Image references** → Extract alt text, note placement
- **Featured image** → Extract description if present at top of article

---

## Metadata Extraction

### From the Article

| Field | Source |
|-------|--------|
| Title | H1 heading (remove year brackets like [2024]) |
| Slug | Generate from title (lowercase, hyphens, no special chars) |
| Excerpt | First paragraph or meta description if provided |
| Category | Infer from content or use provided value |
| Read Time | Word count / 200, rounded up |

### Calculated Fields

**Read Time Formula:**
```
words = count all words in body content
minutes = ceil(words / 200)
format = "{minutes} MIN READ"
```

**Slug Generation:**
```
1. Take title
2. Lowercase
3. Remove special characters
4. Replace spaces with hyphens
5. Remove consecutive hyphens
6. Trim to reasonable length (60 chars max)
```

---

## Process

### Step 1: Read the Source

Read the markdown file and identify:
- H1 title
- All H2 sections
- Meta information (if present at top or bottom)
- Featured image description (if present)
- Sources section

### Step 2: Check Section Count

Count H2 sections (excluding Sources). If >7, plan a consolidation strategy per the constraint above.

### Step 3: Transform Content

1. Extract title and metadata
2. Split content by H2 sections
3. Apply formatting rules
4. Collect sources/links

### Step 4: Generate Output

Create the formatted file with:
- Clear section delimiters
- All metadata fields populated
- Content properly structured
- Sources consolidated

---

## Output

Save the formatted file with the `-formatted.txt` suffix as described in the Arguments and Output Convention above. Place in the same directory as the source file.

---

## Quality Checks

Before delivering formatted content:

- [ ] Title extracted correctly (no markdown artifacts)
- [ ] All H2 sections captured as separate blocks (max 7; consolidated if needed)
- [ ] No broken formatting from transformation
- [ ] Read time calculated
- [ ] Slug is URL-safe
- [ ] Sources section included
- [ ] No empty sections
- [ ] Content can be copy-pasted without further editing

---

## Framer-Specific Notes

- Rich text supports **bold**, *italic*, lists, tables, and basic formatting
- Links should be added manually in the Framer editor
- Images are handled separately through Framer's media library
- Maximum 7 Subtitle/Content pairs (see constraint above)

---

## Related Skills

- **contentwriting**: Write the blog post first
- **humanize**: Polish tone before formatting
- **copy-editing**: Final quality check before formatting
