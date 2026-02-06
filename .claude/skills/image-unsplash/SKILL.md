---
name: image-unsplash
version: 1.1.0
argument-hint: "<article-path> e.g. hyperspell/output/context-engineering-final.md"
description: "Find and download a hero image for a blog post using the Unsplash API. Use when the user says 'unsplash image,' 'find an image on unsplash,' 'hero image unsplash,' or runs /image-unsplash with an article path."
---

# Image Unsplash

## Arguments and Output Convention

- **`$ARGUMENTS`** = path to the article (draft or final, e.g. `hyperspell/output/context-engineering-final.md`)
- **Customer folder**: first path segment of `$ARGUMENTS` (e.g. `hyperspell`)
- **Context file**: glob `{customer-folder}/company-context-*.md` — read Sections 1 and 4 for brand context. If not found, warn the user and proceed with article content only.
- **Output**: image files saved to the same directory as the article
- **Pipeline position**: `/contentwriting` → `/refine` → `/image-unsplash` → `/schema` → `/framer`

---

## Step 0: Parse Arguments & Load API Key

1. Parse `$ARGUMENTS` to get the article path.
2. Derive **customer folder** from the first path segment.
3. Derive **article directory** from the directory containing the article file.
4. Derive **base name** from the input filename by stripping the extension and any known suffix (`-framer`, `-final`, `-draft`, ` (v1)`, etc.). For example:
   - `02-eligibility-verification-framer.txt` → `02-eligibility-verification`
   - `context-engineering-final.md` → `context-engineering`
   - `what-is-ai-agent-memory-draft (v1).md` → `what-is-ai-agent-memory`
   This base name is used for image file naming (see Step 5).
5. Check for the Unsplash API key. First try the env var, then fall back to `.env` file:

```bash
if [ -z "$UNSPLASH_ACCESS_KEY" ] && [ -f .env ]; then
  export $(grep -v '^#' .env | grep UNSPLASH_ACCESS_KEY)
fi
echo $UNSPLASH_ACCESS_KEY
```

If still empty, stop and ask the user:
> Set your Unsplash access key in `.env` (see `.env.example`) or export it directly.
> Free API keys at https://unsplash.com/developers

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

If no YAML frontmatter exists, extract the title from the first H1 (`# ...`) or from structured metadata (e.g. Framer `Title:` field). Infer tags from the content. Warn the user about missing frontmatter.

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
- Unsplash has high-quality editorial photography — lean into real scenes over stock-photo cliches

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

## Step 3: Search Unsplash API

For each query (up to 3), call the Unsplash API:

```bash
curl -s -H "Authorization: Client-ID $UNSPLASH_ACCESS_KEY" \
  "https://api.unsplash.com/search/photos?query={url-encoded-query}&orientation=landscape&per_page=5"
```

Parse the JSON response. For each photo in `results[]`, extract:
- `id`
- `user.name` (photographer)
- `user.links.html` (photographer profile URL)
- `urls.raw` (base URL for custom sizes)
- `urls.regular` (1080px wide — for preview)
- `alt_description`
- `color` (dominant hex color)
- `description` (if available)
- `links.download_location` (required for triggering download per API guidelines)

Collect up to 15 candidate images total across all queries. Deduplicate by `id`.

### Fallbacks

- If a query returns 0 results, move to the next query.
- If all queries return 0 results, tell the user and ask them to provide manual search terms.
- If the API returns an error (401, 403, 429, etc.), report the error clearly:
  - **401**: API key is invalid. Ask user to check their key.
  - **403**: Rate limit hit (50 req/hr for demo apps). Ask user to wait and retry.
  - Other errors: show the status code and message.

---

## Step 4: Present Candidates

Show the user the top 5-8 candidates in a numbered list:

```
Found {n} candidate images. Here are the top picks:

1. "{alt_description}" by {user.name} — color: {color}
   Preview: {urls.regular}

2. "{alt_description}" by {user.name} — color: {color}
   Preview: {urls.regular}

3. ...
```

Then ask the user:
> Pick one or more images by number (e.g. "1" or "1,3"), or ask me to search with different queries.

Wait for user selection before proceeding.

---

## Step 5: Download Selected Image(s)

For each selected image:

### 5.1 Trigger Download Event (Required by Unsplash API Guidelines)

```bash
curl -s -H "Authorization: Client-ID $UNSPLASH_ACCESS_KEY" "{links.download_location}"
```

This is required by the Unsplash API terms. It does not download the image — it notifies Unsplash that a download occurred, which credits the photographer.

### 5.2 Download Images

Use the `urls.raw` base URL with Unsplash's dynamic resizing parameters to get exact sizes. Images are saved **next to the input file** and named using the **base name** derived in Step 0:

```bash
# Full hero (1920px wide, high quality)
curl -sL -o "{article-dir}/{base-name}-hero.jpg" "{urls.raw}&w=1920&q=80"

# OG/social image (1200x627, cropped)
curl -sL -o "{article-dir}/{base-name}-hero-og.jpg" "{urls.raw}&w=1200&h=627&fit=crop&q=80"
```

**Naming convention:**
- `{base-name}-hero.jpg` — full-width hero image (1920px wide)
- `{base-name}-hero-og.jpg` — OG/social image (1200x627)

Where `{base-name}` is derived from the input filename (see Step 0, item 4), and `{article-dir}` is the directory containing the input file.

If the user selects multiple images, number them: `{base-name}-hero-1.jpg`, `{base-name}-hero-og-1.jpg`, etc.

---

## Step 6: Report

After successful download, report to the user:

```
Saved:
- {article-dir}/{base-name}-hero.jpg (1920px, full hero)
- {article-dir}/{base-name}-hero-og.jpg (1200x627, OG/social)

Photographer: {user.name} on Unsplash ({user.links.html})
Unsplash license: free for commercial use. Attribution is appreciated but not required.
```

---

## Constraints

- **No Python dependencies.** Use `curl` + Bash for all API calls.
- **Trigger download endpoint.** The Unsplash API guidelines require calling `links.download_location` when downloading an image. This credits the photographer.
- **Attribution appreciated but not required** by the Unsplash license. Credit the photographer in the report as a courtesy.
- **Rate limit awareness**: Demo apps get 50 requests/hour. Each invocation uses 4-12 requests (3 search + 1-3 download triggers + 2-6 image downloads). Monitor usage if running multiple searches.
- **No watermarks.** Unsplash images are clean at all sizes.
- **Custom sizes via URL params.** Unsplash supports dynamic resizing — append `&w=`, `&h=`, `&fit=crop`, `&q=` to `urls.raw` for exact dimensions.
- **Customer-facing output**: Image files only. No diagnostic files, no internal notes in the article directory.

---

## Fallback Strategies

| Situation | Fallback |
|-----------|----------|
| `UNSPLASH_ACCESS_KEY` not set | Ask user to set it. Link to https://unsplash.com/developers for free signup. |
| No company-context found | Warn user, proceed with article content only for query generation. |
| No YAML frontmatter | Extract title from first H1 or structured metadata, tags from content. Warn user. |
| API returns 0 results for a query | Try next query. If all fail, ask user for manual search terms. |
| API rate limit hit (403) | Tell user to wait and retry. Demo apps: 50 req/hr. |
| API key invalid (401) | Tell user to check their key at https://unsplash.com/developers. |

---

## Related Skills

- **image-pexels**: Alternative image source using Pexels API (different image library)
- **contentwriting**: Write blog posts (run image skills after refine)
- **refine**: Polish drafts to production-ready (run before image skills)
- **schema**: Generate one-time Next.js JSON-LD templates
- **framer**: Format for Framer CMS publishing
- **company-context**: Establish brand voice and context (read by this skill automatically)
