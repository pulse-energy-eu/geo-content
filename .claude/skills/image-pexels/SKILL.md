---
name: image-pexels
version: 1.0.0
argument-hint: "<article-path> e.g. hyperspell/articles/context-engineering-final.md"
description: "Find and download a hero image for a blog post using the Pexels API. Use when the user says 'find an image,' 'hero image,' 'blog image,' 'featured image,' or runs /image-pexels with an article path."
---

# Image Pexels

## Arguments and Output Convention

- **`$ARGUMENTS`** = path to the article (draft or final, e.g. `hyperspell/articles/context-engineering-final.md`)
- **Customer folder**: first path segment of `$ARGUMENTS` (e.g. `hyperspell`)
- **Context file**: glob `{customer-folder}/company-context-*.md` — read Sections 1 and 4 for brand context. If not found, warn the user and proceed with article content only.
- **Output**: image files saved to the same directory as the article
- **Pipeline position**: `/contentwriting` → `/refine` → `/image-pexels` → `/schema` → `/framer`

---

## Step 0: Parse Arguments & Load API Key

1. Parse `$ARGUMENTS` to get the article path.
2. Derive **customer folder** from the first path segment.
3. Derive **article directory** from the directory containing the article file.
4. Check for the Pexels API key. First try the env var, then fall back to `.env` file:

```bash
if [ -z "$PEXELS_API_KEY" ] && [ -f .env ]; then
  export $(grep -v '^#' .env | grep PEXELS_API_KEY)
fi
echo $PEXELS_API_KEY
```

If still empty, stop and ask the user:
> Set your Pexels API key in `.env` (see `.env.example`) or export it directly.
> Free API keys at https://www.pexels.com/api/

Do not proceed until the key is available.

---

## Step 1: Read Inputs

### 1.1 Read the Article

Read the article file. Extract from YAML frontmatter:
- `title`
- `slug`
- `tags`
- `seo.primaryKeyword`
- `seo.secondaryKeywords`
- `category`

Also extract the first 200 words of the article body (after frontmatter) for topic context.

If no YAML frontmatter exists, extract the title from the first H1 (`# ...`) and infer tags from the content. Warn the user about missing frontmatter.

### 1.2 Read Company Context (Optional)

Glob `{customer-folder}/company-context-*.md`. If found, read:
- **Section 1** (Company & Product Overview) — for industry and product context
- **Section 4** (Voice & Tone) — for brand vibe and visual style hints

Note any color palette, visual style, or aesthetic information mentioned.

If no company-context file is found, warn the user and proceed using article content only for query generation.

---

## Step 2: Generate Search Queries

Using the extracted article metadata and company context, generate 3-5 stock photo search queries.

**Query generation rules:**
- Translate abstract concepts into concrete visual scenes or objects
- Each query should be 2-4 words
- Include at least one abstract/artistic option (e.g. "glowing neural network", "connected nodes abstract")
- Include at least one option with people or workspaces (e.g. "developer laptop coding", "team whiteboard planning")
- Prefer landscape-oriented scenes (these work best as hero images)
- Consider the brand's industry and tone from company-context

**Inputs to consider:**
- Article title: `{title}`
- Tags: `{tags}`
- Primary keyword: `{seo.primaryKeyword}`
- First 200 words of article body
- Company industry (from company-context Section 1)

Output a ranked list of 3-5 queries. Show them to the user before searching:

```
Generated search queries:
1. {query1}
2. {query2}
3. {query3}
...
```

---

## Step 3: Search Pexels API

For each query (up to 3), call the Pexels API:

```bash
curl -s -H "Authorization: $PEXELS_API_KEY" \
  "https://api.pexels.com/v1/search?query={url-encoded-query}&orientation=landscape&per_page=5"
```

Parse the JSON response. For each photo in `photos[]`, extract:
- `id`
- `photographer`
- `src.landscape` (1200x627 — for OG/preview)
- `src.large2x` (1920px wide — for full hero)
- `src.original` (full resolution)
- `alt` description
- `avg_color` (hex)

Collect up to 15 candidate images total across all queries. Deduplicate by `id`.

### Fallbacks

- If a query returns 0 results, move to the next query.
- If all queries return 0 results, tell the user and ask them to provide manual search terms.
- If the API returns an error (401, 429, etc.), report the error clearly:
  - **401**: API key is invalid. Ask user to check their key.
  - **429**: Rate limit hit (200 req/hr). Ask user to wait and retry.
  - Other errors: show the status code and message.

---

## Step 4: Present Candidates

Show the user the top 5-8 candidates in a numbered list:

```
Found {n} candidate images. Here are the top picks:

1. "{alt}" by {photographer} — avg color: {avg_color}
   Preview: {src.landscape}

2. "{alt}" by {photographer} — avg color: {avg_color}
   Preview: {src.landscape}

3. ...
```

Then ask the user:
> Pick one or more images by number (e.g. "1" or "1,3"), or ask me to search with different queries.

Wait for user selection before proceeding.

---

## Step 5: Download Selected Image(s)

For each selected image, download two sizes to the article's directory:

```bash
curl -sL -H "Authorization: $PEXELS_API_KEY" -o "{article-dir}/{slug}-hero.jpg" "{src.large2x}"
curl -sL -H "Authorization: $PEXELS_API_KEY" -o "{article-dir}/{slug}-hero-og.jpg" "{src.landscape}"
```

**Naming convention:**
- `{slug}-hero.jpg` — full-width hero image (1920px wide)
- `{slug}-hero-og.jpg` — OG/social image (1200x627)

Where `{slug}` comes from the article's YAML frontmatter `slug` field, and `{article-dir}` is the directory containing the article file.

If the user selects multiple images, number them: `{slug}-hero-1.jpg`, `{slug}-hero-og-1.jpg`, etc.

---

## Step 6: Report

After successful download, report to the user:

```
Saved:
- {article-dir}/{slug}-hero.jpg (1920px, full hero)
- {article-dir}/{slug}-hero-og.jpg (1200x627, OG/social)

Photographer: {name} on Pexels (no attribution required)
Pexels license: free for commercial use.
```

---

## Constraints

- **No Python dependencies.** Use `curl` + Bash for all API calls.
- **No attribution required** by Pexels license, but credit the photographer in the report as a courtesy.
- **Rate limit awareness**: Pexels allows 200 requests/hour. Each invocation uses 3-9 requests. Well within limits.
- **No watermarks.** Pexels images are clean at all sizes.
- **Customer-facing output**: Image files only. No diagnostic files, no internal notes in the article directory.

---

## Fallback Strategies

| Situation | Fallback |
|-----------|----------|
| `PEXELS_API_KEY` not set | Ask user to set it. Link to https://www.pexels.com/api/ for free signup. |
| No company-context found | Warn user, proceed with article content only for query generation. |
| No YAML frontmatter | Extract title from first H1, tags from content. Warn user. |
| API returns 0 results for a query | Try next query. If all fail, ask user for manual search terms. |
| API rate limit hit (429) | Tell user to wait and retry. |
| API key invalid (401) | Tell user to check their key at https://www.pexels.com/api/. |

---

## Related Skills

- **contentwriting**: Write blog posts (run image-pexels after refine)
- **refine**: Polish drafts to production-ready (run before image-pexels)
- **schema**: Generate one-time Next.js JSON-LD templates (run after image-pexels)
- **framer**: Format for Framer CMS publishing
- **company-context**: Establish brand voice and context (read by this skill automatically)
