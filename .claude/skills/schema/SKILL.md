---
name: schema
version: 1.0.0
argument-hint: "<article-path> e.g. hyperspell/articles/context-engineering-final.md"
description: "When the user wants to generate structured data for a blog post. Use when the user says 'schema,' 'generate schema,' 'create JSON-LD,' 'structured data,' or 'schema markup.' Reads YAML frontmatter and article body from a -final.md file, plus company-context, to produce ready-to-paste JSON-LD snippets (Article, FAQPage, Organization, BreadcrumbList)."
---

# Schema — Structured Data Generator

## Arguments and Output Convention

- **`$ARGUMENTS`** = path to the article (e.g. `hyperspell/articles/context-engineering-final.md`)
- **Customer folder**: first path segment of `$ARGUMENTS` (e.g. `hyperspell`)
- **Context file**: glob `{customer-folder}/company-context-*.md` — read Section 8 (Structured Data & Publishing) for org/author data. If not found, warn the user and suggest running `/company-context` first.
- **Output naming**: replace `-final.md` with `-schema.html` in the filename.
- **Next step**: Tell the user: "Paste the contents of `{output-path}` into your CMS `<head>` or Custom Code section. Then run `/framer {article-path}` to format the article body for publishing."

---

You generate ready-to-paste JSON-LD structured data from a refined blog post. This is the step that makes articles fully SEO/GEO-optimized with schema markup that enables rich results in Google and improves citation rates in AI Overviews.

## Pipeline Position

```
/contentwriting → /refine → /schema → /framer (optional)
```

`/schema` runs after `/refine` produces the -final.md with YAML frontmatter. It reads but does not modify the article file.

---

## Step 1: Load Inputs

### 1.1 Read the Article

Read the -final.md file. Extract:

- **YAML frontmatter** — title, slug, author, date, category, tags, seo (metaTitle, metaDescription, primaryKeyword, secondaryKeywords)
- **FAQ section** — find the "Frequently Asked Questions" H2, parse each H3 as a question and the text below it (up to the next H3 or H2) as the answer
- **Article word count** — for the `wordCount` field in Article schema

If the article has no YAML frontmatter, warn the user: "This article has no YAML frontmatter. Run `/refine` on a draft created with contentwriting v3.4.0+ to get frontmatter. You can also add it manually."

### 1.2 Read Company Context

Find and read the company-context file. Extract from Section 8 (Structured Data & Publishing):

- Blog base URL
- Default author (name, role, URL)
- Organization logo URL
- Social profile URLs

Also extract from Section 1 (Company & Product Overview):

- Company name
- Website URL

If Section 8 is missing, warn the user: "Company context is missing Section 8 (Structured Data & Publishing). Run `/company-context` to update it. Generating schemas with available data."

Use whatever data is available. Leave fields empty only if no source provides the data — never fabricate URLs or names.

---

## Step 2: Generate JSON-LD Schemas

Generate four schema blocks. Each must be valid JSON-LD per schema.org specifications.

### 2.1 Article Schema (BlogPosting)

```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "{frontmatter.title}",
  "description": "{frontmatter.seo.metaDescription}",
  "url": "{blogBaseUrl}/{frontmatter.slug}",
  "datePublished": "{frontmatter.date}",
  "dateModified": "{frontmatter.dateModified or frontmatter.date}",
  "author": {
    "@type": "Person",
    "name": "{frontmatter.author or context.defaultAuthor.name}",
    "url": "{context.defaultAuthor.url}",
    "jobTitle": "{context.defaultAuthor.role}"
  },
  "publisher": {
    "@type": "Organization",
    "name": "{context.companyName}",
    "url": "{context.websiteUrl}",
    "logo": {
      "@type": "ImageObject",
      "url": "{context.logoUrl}"
    }
  },
  "keywords": "{frontmatter.tags joined by comma}",
  "wordCount": {articleWordCount},
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "{blogBaseUrl}/{frontmatter.slug}"
  }
}
```

### 2.2 FAQPage Schema

Only generate this if the article has a FAQ section with at least one Q&A pair.

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "{H3 question text}",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "{answer text below the H3, plain text, no markdown}"
      }
    }
  ]
}
```

Strip markdown formatting from answer text (remove `**bold**`, `[links](url)`, etc.) — JSON-LD values must be plain text.

### 2.3 Organization Schema

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "{context.companyName}",
  "url": "{context.websiteUrl}",
  "logo": "{context.logoUrl}",
  "sameAs": ["{socialProfileUrl1}", "{socialProfileUrl2}"]
}
```

Only include `sameAs` URLs that were found in company-context. Do not guess social profile URLs.

### 2.4 BreadcrumbList Schema

```json
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "{context.websiteUrl}"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Blog",
      "item": "{context.blogBaseUrl}"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "{frontmatter.title}",
      "item": "{blogBaseUrl}/{frontmatter.slug}"
    }
  ]
}
```

---

## Step 3: Validate

Before writing output, check:

- [ ] All JSON is valid (no trailing commas, proper quoting)
- [ ] `headline` is under 110 characters (Google truncates longer)
- [ ] `description` is under 160 characters
- [ ] `datePublished` is ISO 8601 format (YYYY-MM-DD)
- [ ] All URLs are absolute (start with `https://`)
- [ ] FAQ answers are plain text (no markdown, no HTML)
- [ ] No empty string values for required fields — omit the field instead

If any check fails, fix it before output. If a required URL is missing (e.g., no blog base URL), note this in a comment in the output.

---

## Step 4: Write Output

Save to `{slug}-schema.html` in the same directory as the article.

### Output Format

```html
<!-- JSON-LD Structured Data for: {article title} -->
<!-- Generated by /schema — paste into CMS <head> or Custom Code section -->

<!-- Article Schema -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  ...
}
</script>

<!-- FAQ Schema -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  ...
}
</script>

<!-- Organization Schema -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  ...
}
</script>

<!-- Breadcrumb Schema -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  ...
}
</script>
```

If a schema could not be generated (e.g., no FAQ section found), omit it and note this to the user.

---

## Constraints

- **Read-only on the article**: Never modify the -final.md file.
- **No fabrication**: Only use data from the frontmatter and company-context. Never invent URLs, author names, or social profiles.
- **Valid JSON**: Every `<script>` block must contain valid JSON. Test by mentally parsing it.
- **Plain text in schema values**: No markdown, no HTML inside JSON-LD string values.

---

## Related Skills

- **contentwriting**: Write blog posts with YAML frontmatter (generates the source data)
- **refine**: Polish drafts and preserve frontmatter (produces the -final.md input)
- **company-context**: Establish brand context including Section 8 schema data
- **framer**: Format the article body for Framer CMS publishing
