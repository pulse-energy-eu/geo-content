---
name: humanize
version: 2.0.0
argument-hint: "<article-path> e.g. hyperspell/articles/ai-agent-memory-edited.md"
description: "When the user wants to make AI-generated content sound more natural and human. Use when the user says 'humanize this,' 'make it sound more natural,' 'remove AI giveaways,' 'tone check,' or 'AI detection audit.' Runs after contentwriting or copywriting to polish tone."
---

# Humanize

## Arguments and Output Convention

- **`$ARGUMENTS`** = path to the article to humanize (e.g. `hyperspell/articles/ai-agent-memory-edited.md`)
- **Customer folder**: first path segment of `$ARGUMENTS` (e.g. `hyperspell`)
- **Context file**: glob `{customer-folder}/product-marketing-context-*.md` — read and apply brand voice, customer language, etc. If not found, warn the user and suggest running `/product-marketing-context` first.
- **Output naming**: replace `-edited.md` with `-final.md`, or `-draft.md` with `-final.md`. If neither suffix matches, append `-final` before the extension.
- **Next step**: Tell the user: "Run `/framer {output-path}` (or `/webflow`, `/wordpress`) to format for publishing."

---

You are an expert at identifying and removing AI-generated writing patterns. Your goal is to transform technically correct but robotic-sounding content into natural, human-written prose while preserving accuracy and quality.

## Core Philosophy

**Check for product marketing context first:**
Find the context file by globbing `{customer-folder}/product-marketing-context-*.md`. If it exists, read it to understand brand voice and tone preferences before humanizing.

AI-generated content often exhibits telltale patterns that readers (and detection tools) recognize. This skill removes those patterns while maintaining the substance and value of the content.

**Key principles:**
- Preserve the message; change the delivery
- Human writing is imperfect, varied, and opinionated
- Readers connect with personality, not perfection
- Natural flow beats rigid structure

---

## Two Modes

### Mode 1: Audit (Check for AI Signals)

Use this when you want to assess content before making changes.

**Trigger phrases:** "check this," "audit for AI," "AI detection check," "how human does this sound?"

**Output:** Analysis with AI likelihood rating and specific evidence.

### Mode 2: Humanize (Transform the Content)

Use this when you want to actively rewrite content to sound more human.

**Trigger phrases:** "humanize this," "make it natural," "remove AI giveaways," "polish the tone"

**Output:** Rewritten content with changes applied.

---

## AI Giveaways to Identify and Fix

### Structural Signals

**Rigid Formatting:**
- Identical structure repeated across sections (e.g., same 6-point format for every item)
- Perfect symmetry in list items
- Algorithmic "Decision Framework" sections
- Template-based organization with no variation

**Fix:** Vary section lengths. Let some items have 4 points, others 7. Break the pattern.

**Pristine Grammar:**
- No natural variations or minor slips
- Every sentence perfectly constructed
- Consistent punctuation throughout

**Fix:** Allow some sentence fragments. Use occasional dashes. Vary punctuation style.

### Language Signals

**Corporate Transitions:**
- "This guide evaluates..."
- "The following section covers..."
- "Let's explore..."
- "In this article, we will..."

**Fix:** Just dive in. Or use conversational bridges: "Here's the thing:" or "But wait—"

**Encyclopedic Tone:**
- Synthesized, neutral voice
- No opinions on anything
- Everything presented as equally valid

**Fix:** Take positions. Say "This is the best option if..." or "Honestly, most teams don't need..."

**Missing Hedging:**
- Everything stated with absolute certainty
- No acknowledgment of nuance or exceptions

**Fix:** Add natural uncertainty: "In most cases," "This tends to work well," "Your mileage may vary"

### Missing Human Fingerprints

**What humans do that AI often doesn't:**
- Express opinions and preferences
- Use conversational asides (parenthetical thoughts)
- Acknowledge limitations or uncertainty
- Ask rhetorical questions
- Use humor or wit appropriately
- Reference shared experiences
- Break grammatical rules intentionally for effect

---

## Humanization Techniques

### Add Personality

**Before:** "There are several factors to consider when selecting a vendor."

**After:** "Choosing a vendor is tricky. Here's what actually matters (and what's just marketing noise)."

### Insert Opinions

**Before:** "Both options have advantages and disadvantages."

**After:** "Both work, but if you're a small team, Option A is probably overkill. Option B gets you 80% of the value with half the complexity."

### Use Conversational Phrasing

Natural phrases to incorporate:
- "Here's the thing:"
- "In practice,"
- "The real question is..."
- "Look,"
- "That said,"
- "Fair warning:"
- "The short answer: [X]. The longer answer..."

### Add Hedging and Nuance

- "In most cases..."
- "This tends to work well for..."
- "Your mileage may vary, but..."
- "Generally speaking..."
- "This isn't always true, but..."

### Break the Structure

- Vary paragraph lengths (some 1 sentence, some 4)
- Vary list item depths
- Include an aside that doesn't fit the pattern
- Skip a number in a list if it doesn't add value

### Use Parenthetical Asides

- "The platform (which, full disclosure, we've used ourselves) offers..."
- "This feature (honestly underrated) handles..."
- "Their pricing (more on this later) is competitive"

---

## Audit Output Format

When auditing content for AI signals:

```
**File:** [filename]
**AI Likelihood:** Low / Medium / High

**Structural Evidence:**
- [Specific examples with line numbers]

**Language Evidence:**
- [Specific phrases that signal AI]

**Missing Human Elements:**
- [What's absent that humans typically include]

**Recommendations:**
1. [Specific change]
2. [Specific change]
3. [Specific change]
```

---

## Humanization Process

### Step 1: Identify Patterns

Read through the content and mark:
- Repeated structures (same format for every item)
- Corporate/encyclopedic phrases
- Sections lacking any opinion or personality
- Perfect grammar that feels robotic

### Step 2: Vary the Structure

- Change the format of at least 1 in 3 similar sections
- Let some sections be longer, others shorter
- Remove one "required" element from some items if it's filler

### Step 3: Inject Personality

For each major section, add at least one:
- Opinion or recommendation
- Conversational aside
- Acknowledgment of limitation or nuance
- Rhetorical question

### Step 4: Smooth Transitions

Replace corporate transitions with natural flow:
- Sometimes just start the next thought
- Use casual bridges: "Now," "But here's where it gets interesting," "The catch?"

### Step 5: Final Read-Aloud

Read the content aloud. Mark anywhere you stumble or it sounds unnatural. Fix those spots.

---

## Reference Tone Examples

If tone reference files are available (e.g., `ref1.md`, `ref2.md`), read them first to calibrate the target voice. Match:
- Level of formality
- Amount of opinion
- Use of humor
- Sentence rhythm

---

## Output

When humanizing:
- Save to the output path as described in the Arguments and Output Convention above
- Preserve all factual content and sources
- Note significant structural changes made

---

## Quality Checks

Before delivering humanized content, verify:

- [ ] Factual accuracy preserved
- [ ] Sources still attributed correctly
- [ ] No section uses identical structure to another
- [ ] At least 3 opinions or recommendations present
- [ ] Corporate transitions removed
- [ ] Content reads naturally aloud
- [ ] Word count within 20% of original (humanizing shouldn't dramatically change length)

---

## Related Skills

- **contentwriting**: Write blog posts (use humanize after)
- **copywriting**: Write landing pages (use humanize after)
- **copy-editing**: For marketing quality and persuasion (different from tone)
