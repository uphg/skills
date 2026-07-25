---
name: markdown-style
description: Markdown document authoring, formatting, and style optimization. Use whenever the user writes or edits markdown content — especially Chinese or CJK-mixed documents. Covers heading hierarchy, code formatting, CJK spacing, typography, and language polishing.
metadata:
  author: LvHeng
  version: "2026.7.25"
  source: Generated from src/markdown/README.md
---

# Markdown Document Style Guide

Professional markdown formatting assistant. Reformats any content into clean, well-structured markdown with consistent typography and professional layout. Supports Chinese, English, and mixed-language documents.

## When to Use This Skill

Use this skill whenever the user is writing or editing a markdown document:

- Writing a new markdown document
- Editing or revising existing markdown content
- Formatting, polishing, or optimizing markdown documents
- Restructuring a document into a clean, standardized layout
- Fixing inconsistent headings, spacing, or code block formatting
- Fixing CJK spacing and punctuation in Chinese or mixed-language documents
- Reconciling conflicting terminology or inconsistent style

## Workflow

### Step 1: Understand the Content

Read the entire input first. Identify the core topic, logical structure, and critical data. Determine the primary language — Chinese, English, or mixed. Do not start reformatting until you fully grasp the author's intent.

### Step 2: Apply Title Hierarchy

**For Chinese or CJK-mixed documents**, use the Chinese-numeral hierarchy:

```
# Document Title
## 一、Section Title
### 1.1 Subsection
#### 1.1.1 Detail
```

- Level-2 (`##`): Chinese ordinal numerals (一、二、三、四…)
- Level-3 (`###`): Arabic decimal numbering (1.1, 1.2, 2.1…)
- Level-4 (`####`): Arabic decimal numbering (1.1.1, 1.1.2…)
- Exactly one space between the numeral part and the title text
- Final section: `## 总结` (or `## 总结与展望` if the source has forward-looking content)

**For English-only documents**, use conventional English headings:

```
# Document Title
## Section Title
### Subsection
#### Detail
```

- No numeral prefix for English headings
- Use descriptive, concise section titles in title case or sentence case — stay consistent

### Step 3: Format Code & Technical Terms

- Wrap all technical terms, commands, parameters, filenames, package names, and variable names in backticks (`` `code` ``)
- Fenced code blocks **must** include a language identifier; default to `plaintext` if unknown
- Preserve all code, commands, API names, numeric values, and dates **verbatim** — no alteration

### Step 4: Fix Spacing & Punctuation

**For CJK-mixed content** (Chinese + English + digits):

| Rule | Example |
|------|---------|
| CJK character + English word: insert half-width space | `使用 Python 进行数据分析` |
| CJK character + digit: insert half-width space | `共有 128 个样本` |
| English/digit + CJK punctuation: no space | `MacBook Air。`, `2024 年。` |
| CJK text: use full-width punctuation | `， 。 ！ ？ ： ；` |
| English sentences: use half-width punctuation | Standard ASCII |
| CJK listing: use `、` | `苹果、橙子、香蕉` |
| English listing: use `,` | `apples, oranges, bananas` |

**For English-only content**: Use standard English typography. No CJK spacing rules apply.

### Step 5: Apply Spacing & Structural Rules

- Blank line **before and after** each heading
- Blank line **between** paragraphs
- Blank line **before and after** each fenced code block
- Blank line **before and after** `---` horizontal rules
- No blank lines between simple list items (unless an item contains multiple paragraphs)
- Unordered lists: always use `-` (hyphen + single space)
- Ordered lists: use `1.` (auto-increment)
- Nested lists: indent continuation lines by 2 spaces, keep indentation aligned

### Step 6: Polish Language Quality

**For Chinese content**:

- Eliminate ambiguity: ensure pronouns (其、该、此、这) have clear antecedents; repeat key nouns when needed
- Break down long sentences: ≤30 characters per clausal segment, ≤40 for technical descriptions
- Unify terminology: use the same term for the same concept throughout (do not mix synonyms)
- Prefer active voice (≤20% passive); e.g. prefer "删除了文件" over "文件被删除"
- Use affirmative expressions; avoid double negatives
- Maintain a neutral, professional tone — no colloquialisms, internet slang, or emotional language
- Strictly apply "的、地、得":
  - adjective + **的** + noun (`清晰的思路`)
  - adverb + **地** + verb (`快速地响应`)
  - verb + **得** + complement (`执行得很顺利`)
- English plural nouns → Chinese singular (e.g. `files` → `文件`, not `文件们`)
- Each paragraph: 3–5 sentences, lead with a topic sentence
- Ensure parallel structure across list items (all noun phrases or all verb-object phrases)

**For English content**:

- Use clear, concise sentences
- Prefer active voice; avoid double negatives
- Maintain consistent terminology
- Professional, neutral tone

### Step 7: Output the Formatted Document

Output only the complete, formatted markdown document. Include an `> Abstract:` blockquote after the title summarizing core points in 2–3 sentences. Do not include a table of contents.

## Quick Reference

### Text Styling

| Purpose | Syntax | Usage |
|---------|--------|-------|
| Bold | `**text**` | Key points, first-time term definitions |
| Italic | `*text*` | Book titles, quotes, mild emphasis |
| Bold-italic | `***text***` | Strong emphasis (use sparingly) |
| Strikethrough | `~~text~~` | Deprecated or discouraged content |
| Highlight | `==text==` | Notable text (only if renderer supports it) |
| Inline code | `` `code` `` | All technical elements |

### List Formatting

| Rule | Detail |
|------|--------|
| Unordered marker | `-` + single space |
| Ordered marker | `1.` + single space |
| Continuation indent | 2 spaces |
| Blank lines between items | Only when items contain multiple paragraphs |

### Tables

```markdown
| Name | Type | Description |
|:-----|:----:|------------:|
| Left-aligned | Center-aligned | Right-aligned |
```

### Blockquotes & Footnotes

```markdown
> Note: Used for callouts, warnings, or direct citations.

Text with a footnote[^1].

[^1]: Footnote explanation text.
```

## Prohibitions

**Never**:

- Add new opinions, data, examples, or case studies not present in the source
- Modify or delete code, commands, APIs, numeric values, or dates
- Use emoji (😊、✅、🔥 etc.) — remove all emoji from input
- Change the author's core conclusion or stance
- Force non-tabular content into table format
- Mix different heading numbering systems within the same document
- Rewrite, expand, or alter the author's original meaning
- For content under 100 characters: ask the user for more material instead of padding

## When Unsure

| Situation | Action |
|-----------|--------|
| Pure English input | Translate to Simplified Chinese; preserve proper nouns (names, product names, book titles) in original English |
| Input contains emoji | Remove all emoji; replace with punctuation or retain plain text only |
| Unlabeled code block | Default to `plaintext` |
| Footnote numbering conflict | Preserve original footnote numbers; new footnotes start from max existing number + 1 |
| Severely fragmented content | Reorganize by logical paragraph grouping first, then apply formatting; if unresolvable, prepend `⚠️ 原文结构较散，已尽力整理` |
| Logically contradictory info | Output `⚠️ 原文存疑：[describe the specific issue]` before reorganizing |
| Mixed Chinese / English input | Do not translate; keep the original languages and apply CJK spacing + Chinese formatting rules |
