---
name: science-of-reading
description: "Evidence-based literacy support aligned to SoR research."
version: 0.1.0
author: Hermes
metadata:
  hermes:
    tags: [Literacy, Science-of-Reading, Education, K-5]
---

# Science of Reading Agent

Evidence-based literacy skill aligned to the Science of Reading (SoR) research. Validates instructional content against What Works Clearinghouse and Best Evidence Encyclopedia findings. Supports vocabulary selection, Lexile targeting, phonemic awareness, phonics, fluency, vocabulary, and comprehension — the five pillars of reading.

Load `what-works-clearinghouse` and `best-evidence-encyclopedia` alongside this skill for full evidence-based validation.

## When to Use

- "Is this text appropriate for [grade level] readers?"
- "What vocabulary words should I target for Lexile [X]?"
- "Check this passage for SoR alignment"
- "Suggest decodable text for early readers"
- "Analyze this children's book for literacy best practices"

## Prerequisites

- `what-works-clearinghouse` skill (for WWC evidence)
- `best-evidence-encyclopedia` skill (for BEE ratings)
- Pandoc installed for text analysis

## Quick Reference

| Pillar | Research Basis | Implementation |
|---|---|---|
| Phonemic Awareness | WWC Practice Guide: Foundational Skills | Rhyme, alliteration, sound isolation |
| Phonics | BEE Reading/Elementary | Decodable text, sound-symbol mapping |
| Fluency | WWC: Repeated Reading | Page-turning cadence, read-aloud pacing |
| Vocabulary | WWC: Explicit Instruction | Contextual word introduction, tiered vocabulary |
| Comprehension | WWC: Question Generation | Parent prompts, embedded questions |

## Procedure

### 1. Analyze Text Complexity

Load the manuscript and run Lexile analysis:

```bash
pandoc manuscript.md -t plain | python3 -c "
import re, sys
text = sys.stdin.read()
words = re.findall(r'\b\w+\b', text.lower())
sentences = re.split(r'[.!?]+', text)
unique = len(set(words))
avg_sentence = len(words) / max(len(sentences), 1)
print(f'Words: {len(words)} | Unique: {unique}')
print(f'Avg sentence length: {avg_sentence:.1f}')
print(f'Estimated Lexile: {int(200 + avg_sentence * 12 - (unique/len(words))*100)}L')
"
```

### 2. Validate Vocabulary

Check that vocabulary words match SoR tier framework:
- **Tier 1**: Common words (dog, river, park) — should dominate early chapters
- **Tier 2**: Academic words (observe, investigate, protect) — scaffolded through context
- **Tier 3**: Domain-specific (tributary, conservation, ecosystem) — introduced with definitions

### 3. Check Decodability

For K-2 readers, ensure early chapters have high percentage of decodable words:

```bash
grep -o '\b\w\w\w\w*\b' text.txt | sort | uniq -c | sort -rn | head -20
```

### 4. Align with SoR Pillars

For each chapter, verify coverage of the five pillars:

| Chapter | Phonemic Awareness | Phonics | Fluency | Vocabulary | Comprehension |
|---|---|---|---|---|---|

## Pitfalls

- **Lexile ≠ quality** — A low Lexile doesn't mean low quality; it means accessibility
- **Decodable ≠ boring** — Rich stories can still use decodable text
- **SoR is not one method** — It's a body of research, not a specific curriculum

## Verification

```bash
# Verify WWC and BEE skills are available
skill_view(name='what-works-clearinghouse') && echo "WWC ready"
skill_view(name='best-evidence-encyclopedia') && echo "BEE ready"
```