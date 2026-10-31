---
name: science-of-reading
description: "Check K-3 texts and lessons against Science of Reading research using the sor-mcp-server tools: scope-aware decodability, three-cueing detection, vocabulary tiers, screening profiles, and Georgia standards."
version: 0.2.0
author: Hermes
metadata:
  hermes:
    tags: [Literacy, Science-of-Reading, Education, K-3, Decodable-Text]
---

# Science of Reading Agent

Use the **science-of-reading MCP server** ([kosburn0408/sor-mcp-server](https://github.com/kosburn0408/sor-mcp-server)) for every claim about a text. Its tools are deterministic and show their working, so the agent reports what the tools found rather than estimating.

Load `what-works-clearinghouse` and `best-evidence-encyclopedia` alongside this skill when the user asks what the research says about a program or practice.

## When to Use

- "Can a first grader in unit 3 read this passage?"
- "Write a decodable story for grade 1, unit 5"
- "Check this lesson script for three-cueing prompts"
- "Which Tier 2 words in this read-aloud should I pre-teach?"
- "Which Georgia standard does this phonics lesson address?"
- "Review this children's book manuscript for early readers"

## Prerequisites

- The `science-of-reading` MCP server is connected (see the repo README for config).
- For results that match a district's program, the server's `SOR_SCOPE_PATH` points at that program's scope & sequence. Otherwise the server uses an illustrative default sequence; say so in the answer.

## Tools

| Need | Tool | Notes |
|---|---|---|
| What students have been taught | `get_phonics_scope(grade_level, unit)` | Cumulative patterns and heart words |
| Can students decode this text? | `verify_decodable_text(text, grade_level, unit, target_pct=90, min_decodable_pct=None)` | Per-word status, missing patterns, irregular words, three-cueing flags |
| How a word maps to sounds | `map_orthography(words)` | e.g. `ch - a - t → /ch/ /ă/ /t/` |
| Rough difficulty of longer text | `estimate_readability(text)` | Flesch-Kincaid. Not a Lexile measure, and not meaningful for K-2 text |
| Vocabulary to pre-teach | `classify_vocabulary(text)` | Beck Tier 1/2/3 |
| Screening profile | `evaluate_simple_view(decoding_score, language_comp_score, cutoff, diagnosed_skills)` | Screening, not diagnosis |
| Intervention script | `get_instructional_remediation(deficit_code)` | I Do / We Do / You Do |
| Standards | `align_standards(description, state="GA", grade)` | Georgia: current K-12 ELA via SuitCASE |
| Research | `search_evidence(topic)` | Small curated set; check sources before citing |
| Remove student identifiers | `sanitize_pii(record)` | Run before any student data reaches the model |

## Procedure

### 1. Check a passage

1. Ask for (or infer and state) the grade and unit the students have reached.
2. Call `verify_decodable_text`. Report:
   - `decodable_pct` and `accessible_pct` against `target_pct` (and `min_decodable_pct` if the district uses one)
   - `missing_patterns`: the untaught pattern behind each blocked word, so the teacher can swap words or teach the pattern first
   - `irregular_words` to pre-teach as heart words or replace
   - `not_in_dictionary_words` (usually names) for the teacher to check
   - any `cueing_flags`, with the suggested replacement prompts

### 2. Write a decodable passage

Use the `generate_aligned_decodable` prompt, or follow the same loop: get the scope, draft, call `verify_decodable_text`, replace every flagged word, and verify again until `meets_target` is true. Never call a passage decodable without a passing result.

### 3. Review a children's book manuscript

For each chapter: run `verify_decodable_text` at the intended grade/unit (early-reader books), `classify_vocabulary` for Tier 2 words worth a parent prompt, and `estimate_readability` only for chapters long enough (100+ words) and above grade 2.

### 4. Align to standards

Call `align_standards(description, "GA", grade)`. Read the `notices`: they say whether the codes are current (SuitCASE) or legacy.

## Pitfalls

- **Decodability depends on the scope.** The same passage is decodable in unit 8 and not in unit 3. Always state the grade, unit and scope source.
- **Readability formulas are not decodability.** Don't use Flesch-Kincaid or Lexile to judge K-2 text.
- **Screening is not diagnosis.** Never say a student "has dyslexia" from `evaluate_simple_view`.
- **No three-cueing.** Don't suggest "look at the picture", "what would make sense", or "skip it and come back".
- **SoR is a body of research, not one program.**

## Verification

- `get_phonics_scope("1", "3")` returns `status: ok` with taught patterns.
- `verify_decodable_text("Tomorrow the beautiful knight laughed loudly.", "2", "8")` does not meet the target and lists `beautiful` and `laughed` as irregular.
