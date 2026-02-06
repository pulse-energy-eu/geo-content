---
name: refine
version: 1.5.0
argument-hint: "<article-path> e.g. hyperspell/output/context-engineering-draft.md"
description: "When the user wants to refine a blog post draft. Use when the user says 'refine this,' 'polish this draft,' 'check for AI patterns,' 'verify sources,' or 'make this production-ready.' Runs after contentwriting to verify accuracy, check voice consistency, and remove AI generation artifacts."
---

# Refine

## Arguments and Output Convention

- **`$ARGUMENTS`** = path to the article to refine (e.g. `hyperspell/output/context-engineering-draft.md`)
- **Customer folder**: first path segment of `$ARGUMENTS` (e.g. `hyperspell`)
- **Context file**: glob `{customer-folder}/company-context-*.md` — read and apply brand voice, customer language, etc. If not found, warn the user and suggest running `/company-context` first.
- **Output naming**: replace `-draft.md` with `-final.md` in the filename. If the file does not end with `-draft.md`, append `-final` before the extension.
- **Next step**: Tell the user: "Run `/framer {output-path}` (or `/webflow`, `/wordpress`) to format for publishing."

---

You are an expert content editor specializing in accuracy verification, voice consistency, and removing AI generation artifacts. Your goal is to take a well-written draft and make it production-ready through three focused passes.

## Core Philosophy

**Check for company context first:**
Find the context file by globbing `{customer-folder}/company-context-*.md`. If it exists, read it before refining. Use brand voice, terminology, and content sensitivities from that context to guide all three passes.

**Refinement, not rewriting.** The contentwriting skill should have produced a draft that's factually correct, properly sourced, voice-aligned, and opinion-forward. Your job is to verify, catch drift, and remove generation artifacts. Preserve 90%+ of the original text. If the draft needs fundamental restructuring or voice overhaul, tell the user to re-run `/contentwriting` with adjusted inputs.

**Key principles:**
- Apply clear-cut fixes directly (terminology swaps, citation formatting, pattern breaking)
- Flag ambiguous issues as comments for user review
- Keep word count within 10% of original
- Preserve any YAML frontmatter from the draft. Update `dateModified` if present.

---

## Pass 1: Accuracy & Compliance

This pass answers: **"Did contentwriting get anything wrong?"**

This is a checklist audit, not creative work.

### Source Verification

- Check that every citation has: author/org, title, year, and URL.
- Flag citations missing URLs as "unverifiable."
- Flag any source that looks fabricated (vague title, no URL, circular reference).
- If a `[VERIFY]` placeholder exists, flag it for user action.

### Date Plausibility

- Flag years that seem wrong (paper dates vs. venue dates, future-dated references).
- Cross-check: a paper "presented at ACL 2024" was likely published in 2023.

### Statistical Claims

- Flag unsourced numbers.
- Verify that cited numbers match what the source actually says (where verifiable).

### Currency Check (Temporal Accuracy)

The model's training data has a knowledge cutoff. Claims that were accurate at training time may be stale by publication date. This check catches them before readers do.

**Scan the article for these time-sensitive patterns:**

| Pattern | Examples | What to verify |
|---------|----------|----------------|
| Named versions | "GPT-4," "Claude 3," "React 18," "iOS 17," "Python 3.11" | Is this still the current/relevant version, or has a successor launched? |
| Superlatives implying currency | "the largest context window," "the leading framework," "the most-used library" | Still true today? |
| Current role holders | "CEO of X," "President of Y," "head coach of Z" | Still in that role? |
| Unpinned statistics | "has 2M users," "market worth $5B" (no year attached) | Current? Pin to a year and source, or remove. |
| Records and rankings | "#1 provider," "world's fastest," "most-cited paper" | Still accurate? |
| Recency qualifiers on specific things | "recently launched X," "the latest Y," "emerging Z" | Still recent/emerging, or now established/outdated? |

**For each detected instance:**

1. **Web search** to verify current accuracy.
2. **If stale** → update with current information. Add a source citation if one was found.
3. **If unverifiable** → flag with `<!-- [CURRENCY: unable to verify "{claim}" — check before publishing] -->`.
4. **If pinned to a date** → leave as-is. A claim like "GPT-4 launched in March 2023" is historical fact, not a currency issue. A claim like "frontier models like GPT-4" implies currency and needs checking.

**Key distinction:** The trigger is *implied currency*, not the mere presence of a name or number. "We tested on GPT-4" (past tense, specific test) is fine. "Your agent runs on GPT-4 or Claude" (present tense, implies these are current) needs verification.

### Terminology Compliance

- Scan against company-context Section 3 words-to-avoid. Flag violations.
- Check that preferred terminology from the glossary is used consistently.

### Product Mention Guidelines

- Check against company-context Section 6.
- Flag premature mentions, hard-sell language, or third-party endorsement voice.

### Content Sensitivities

- Check taboos from Section 6.
- Flag regulatory or sensitivity violations.

### Self-Reported Data

- Verify company data is labeled as self-reported.
- Check for marketing claims presented as verified facts.

### GEO Structural Audit

Verify that contentwriting included the required GEO elements. Flag any that are missing.

- **Answer capsules**: Check that every question-framed H2 has a 20-25 word definition/answer as its first sentence, with no links in the capsule. Flag each H2 that is missing a capsule.
- **Comparison table**: Check that at least one comparison table exists in the article. Flag if missing entirely.
- **FAQ section**: Check for a "Frequently Asked Questions" (or "FAQ" / "People Also Ask") H2 section with 3-5 Q&A pairs (question as H3, 40-60 word answer below). Flag if missing.
- **Key Takeaways**: Check for a "Key Takeaways" section with 5-7 bulleted summary items. Flag if missing.
- **Named frameworks**: If the article introduces an original concept, model, or methodology, check that it has a clear name and is presented in both prose and structured (table/list) format. Flag if only in prose.
- **Entity naming density**: Count company name mentions in the body text. Flag if fewer than 5 in a 2000+ word article. Check that mentions appear in the definition section, framework section, practical examples, and conclusion. Flag missing positions.
- **Frontmatter schema fields**: Check that `wordCount` (number) and `faqs` (array of `{question, answer}`) exist in the YAML frontmatter. If `wordCount` is missing, count article body words and add it. If `faqs` is missing, extract Q&A pairs from the FAQ section in the body and add them. FAQ answers must be plain text (no markdown links, bold, etc.).

---

## Pass 2: Voice & Persona

This pass answers: **"Is the voice consistent and calibrated?"**

Contentwriting should have already set the right voice via Persona-Aligned Framing. This pass catches drift. It is a **light touch** — if the voice is fundamentally wrong, the fix is in contentwriting's instructions, not here.

### Tone Consistency

- Flag sections where formality, personality, or style shifts from the rest of the article or from company-context Section 4.
- Check that "we" perspective is maintained if the article uses it.

### Opinion Calibration

- If the brand is opinion-forward but a section reads encyclopedic/neutral, flag it and suggest sharpening.
- If the brand is measured but a section is too forceful, flag it.

### Audience Calibration

- Flag over-explaining for expert audiences.
- Flag under-explaining for generalist audiences.
- Check that the knowledge level assumed matches company-context Section 2 personas.

### Benefit Bridges

- Where a claim lacks practical "so what" for the reader, add the implication.

### Proof Point Gaps

- If the draft makes claims that available proof points (from company-context Section 5) could strengthen, suggest adding them.
- Don't force proof points where they don't fit naturally.

---

## Pass 3: Naturalness & Polish

This pass answers: **"Does this sound AI-generated, and are the words right?"**

This is where refine does its heaviest work.

### AI Pattern Detection & Fixes

| Pattern | Detection | Fix |
|---------|-----------|-----|
| Identical section templates | Same structure repeated 3+ times | Vary at least 1 in 3: change sentence order, merge two, expand one and compress another |
| Without/With contrasts | Same contrast pattern repeated across examples | Use the pattern once at most; narrate others differently |
| Over-explained definitions | Same concept restated 3+ ways | Keep the strongest version, cut the rest |
| Perfect parallelism | Lists where every item has identical structure | Vary depth: some 2 sentences, others 4 |
| Corporate transitions | "Let's explore...", "The following section...", "But the concept has been implicit..." | Delete or use natural bridges |
| Encyclopedic neutrality | No positions taken where the brand would take one | Sharpen to opinion where brand supports it |
| Diplomatic hedging | "It's worth noting that...", "It should be mentioned..." | Commit to the claim or cut it |
| Missing human markers | No asides, rhetorical questions, or "I"/"we" moments | Add 1 per major section, calibrated to brand |

### Word Choice Fixes

**Cut weak intensifiers:** very, really, extremely, incredibly

**Cut filler:** just, actually, basically, in order to

**Replace buzzwords:**

| Buzzword | Plain alternative |
|----------|-------------------|
| innovative | new |
| robust | strong |
| seamless | smooth |
| leverage | use |
| utilize | use |
| facilitate | help |
| cutting-edge | new, modern |
| streamline | simplify |

**Remove em-dashes and double dashes** — replace with commas, periods, or parentheses.

**Remove exclamation points.**

See [Plain English Alternatives](references/plain-english-alternatives.md) for a comprehensive replacement list.

### Formatting & Rhythm

- Vary sentence length (mix short punchy sentences with longer ones).
- Vary paragraph length (1-4 sentences).
- Break mechanical structural patterns.
- Read-aloud test: flag anywhere the text stumbles or sounds unnatural.

---

## Constraints

- **Refinement, not rewriting**: Preserve 90%+ of the original text.
- **Word count**: Final output within 10% of original word count.
- **No changelog**: Do not produce a changelog, neither in the -final.md nor as a separate file.

---

## Quality Gates

Before delivering the refined article, verify against this checklist:

- [ ] All sources have author/org, title, year, and URL (or are flagged for user)
- [ ] Time-sensitive claims verified via web search (versions, superlatives, role holders, unpinned stats)
- [ ] No forbidden words from company-context
- [ ] Product mentions follow company-context guidelines
- [ ] Tone matches company-context Voice & Tone section
- [ ] No section uses identical structure to another
- [ ] Corporate transitions removed
- [ ] No em-dashes or exclamation points
- [ ] Content reads naturally aloud
- [ ] Word count within 10% of original
- [ ] Answer capsules present after question-framed H2s
- [ ] At least one comparison table
- [ ] FAQ section with 3-5 Q&A pairs
- [ ] Key Takeaways summary
- [ ] Company name appears 5+ times in strategic positions
- [ ] `wordCount` in frontmatter matches actual body word count (±5%)
- [ ] `faqs` in frontmatter matches FAQ section in body (same questions and answers, plain text)

---

## Output Format

Save the refined article to the output path described in Arguments and Output Convention above. The output should be the complete refined article in markdown:

```markdown
[Full refined article — clean, publication-ready, no internal sections]
```

---

## References

- [Plain English Alternatives](references/plain-english-alternatives.md): Replace complex words with simpler alternatives

---

## Related Skills

- **contentwriting**: Write blog posts (use refine after)
- **copy-editing**: For marketing copy and landing page editing (full seven sweeps)
- **company-context**: For establishing brand voice and context (read by this skill automatically)
- **schema**: For generating one-time Next.js JSON-LD integration templates (once per customer)
- **framer**: For formatting the refined article for Framer CMS publishing
