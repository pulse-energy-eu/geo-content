# Convert Markdown Article to Framer CMS Format

Convert the provided markdown article into a structured format ready for Framer CMS copy-paste.

## Input
$ARGUMENTS (path to markdown file)

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

## Instructions

1. Read the markdown file at the provided path
2. Count H2 sections; if >7, plan consolidation strategy
3. Extract and transform into this exact format:

```
================================================================================
FRAMER CMS IMPORT - [Article Title]
================================================================================

------ METADATA ------
Title: [Extract from H1, remove year bracket if present]
Slug: [Generate URL-friendly slug from title]
Date: [Today's date in DD.MM.YYYY format]
Category: [Suggest: "AI in Healthcare" or "Patient Access Trends"]
Read Time: [Calculate: word count / 200, round up, format as "X MIN READ"]
Featured: No
Featured Image Description: [Extract from top of file if present, otherwise suggest]

------ SUBTITLE MAIN ------
[First H2 heading text]

------ CONTENT 1 ------
[All content under first H2, until next H2]
[Convert markdown formatting:]
- **bold** stays as **bold** (Framer supports it)
- Keep bullet lists as-is
- Convert tables to simple format or note "[TABLE - see source]"
- Remove markdown links, keep text: [text](url) → text
- H3 subheadings become bold lines: ### Heading → **Heading**

------ SUBTITLE 2 ------
[Second H2 heading text]

------ CONTENT 2 ------
[All content under second H2]

[Continue pattern for each H2 section...]

------ SUBTITLE N ------
[Nth H2 heading]

------ CONTENT N ------
[Content under Nth H2]

------ SOURCES (for reference, may need separate handling) ------
[List sources at the end]

================================================================================
END OF ARTICLE
================================================================================
```

## Formatting Rules

1. **H2 headings** → Become SUBTITLE fields
2. **H3 headings** → Become **bold text** within content
3. **Tables** → Preserve as markdown tables (Framer rich text supports tables)
4. **Bold/Italic** → Keep as-is
5. **Bullet lists** → Keep as-is
6. **Links** → Convert `[text](url)` to just `text` (add URLs to sources)
7. **Blockquotes** → Keep as-is
8. **Code blocks** → Keep as-is

## Output
Write the formatted output to a new file in the same directory with `-framer.txt` suffix.
