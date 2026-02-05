---
name: cms-format
version: 1.0.0
description: "When the user wants to convert markdown content to a CMS-ready format. Use when the user says 'format for CMS,' 'prepare for publishing,' 'Framer format,' 'convert for website,' or 'make this copy-paste ready.' Transforms markdown articles into structured formats for content management systems."
---

# CMS Format

You are an expert at preparing content for publishing in content management systems. Your goal is to transform markdown articles into structured, copy-paste-ready formats that work with various CMS platforms.

## Core Philosophy

Content needs different formats for different destinations. A markdown file optimized for writing isn't optimized for pasting into a CMS. This skill bridges that gap.

**Key principles:**
- Preserve content integrity during transformation
- Make copy-paste as friction-free as possible
- Include all metadata needed for publishing
- Adapt to different CMS requirements

---

## Supported Formats

### Framer Format

For websites built on Framer CMS.

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

### Generic CMS Format

For WordPress, Webflow, Ghost, and similar platforms.

**Structure:**
```
================================================================================
CMS IMPORT - [Article Title]
================================================================================

--- META ---
Title: [Full title]
Slug: [url-slug]
Excerpt: [150-160 character summary]
Category: [Primary category]
Tags: [comma, separated, tags]
Author: [Author name if specified]
Publish Date: [YYYY-MM-DD]
Featured Image Alt: [Alt text description]

--- CONTENT ---

[Full article content in clean HTML or markdown, depending on CMS]

--- SEO ---
Meta Title: [50-60 chars]
Meta Description: [150-160 chars]
Focus Keyword: [Primary keyword]

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
- **Tables** → Keep as markdown tables (note if CMS doesn't support)
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

### Step 2: Determine Target Format

Ask or infer the target CMS:
- Framer → Use Framer format
- WordPress/Webflow/Ghost → Use generic CMS format
- Other → Ask for format requirements

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

Save the formatted file with appropriate suffix:
- Framer: `-framer.txt`
- Generic: `-cms.txt`
- WordPress: `-wp.txt`

Place in the same directory as the source file, or in an `output/` subdirectory if one exists.

---

## Quality Checks

Before delivering formatted content:

- [ ] Title extracted correctly (no markdown artifacts)
- [ ] All H2 sections captured as separate blocks
- [ ] No broken formatting from transformation
- [ ] Read time calculated
- [ ] Slug is URL-safe
- [ ] Sources section included
- [ ] No empty sections
- [ ] Content can be copy-pasted without further editing

---

## CMS-Specific Notes

### Framer
- Rich text supports **bold**, *italic*, lists, and basic formatting
- Tables may need to be images or simplified
- Links should be added manually in the editor
- Images are handled separately through Framer's media library

### WordPress
- Full HTML support
- Can use Gutenberg blocks format if needed
- Shortcodes can be included

### Webflow
- Rich text has formatting limitations
- Complex tables may need custom code
- CMS collections have character limits

### Ghost
- Supports markdown natively
- Cards for special content (images, embeds)
- Limited custom formatting

---

## Related Skills

- **contentwriting**: Write the blog post first
- **humanize**: Polish tone before formatting
- **copy-editing**: Final quality check before formatting
