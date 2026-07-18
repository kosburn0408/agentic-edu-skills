---
name: best-evidence-encyclopedia
description: "Search Johns Hopkins BEE for proven education program reviews."
version: 0.1.0
author: Hermes
metadata:
  hermes:
    tags: [BEE, CRRE, Research, Evidence, Education]
---

# Best Evidence Encyclopedia (BEE)

The Best Evidence Encyclopedia is a free resource from Johns Hopkins University's Center for Research and Reform in Education (CRRE), founded by the late Robert Slavin. It provides program reviews and meta-analyses of education interventions across reading, math, science, writing, early childhood, and more — with effect sizes and evidence strength ratings. Complements the federal WWC with a university-based, practitioner-focused approach.

Does NOT cover raw literature searches — focused on synthesized program effectiveness ratings. No API; content is accessed through the WordPress-based site at `https://bestevidence.org`.

## When to Use

- "Does [program] have evidence of effectiveness?"
- "What are the top-rated reading programs for elementary?"
- "Find effect sizes for math interventions"
- "What does the BEE say about tutoring programs?"
- "Evidence for ESSA — is [curriculum] eligible for Title I?"

## Prerequisites

- None — public resource, no login required
- Sister sites for expanded search:
  - `https://evidenceforessa.org` — ESSA-aligned evidence ratings
  - `https://proventutoring.org` — Evidence on tutoring programs
  - `https://bestevidence.org/brief/` — Best Evidence in Brief newsletter

## How to Run

1. Navigate `browser_navigate` to `https://bestevidence.org`
2. Browse Program Reviews by subject (Reading, Math, Science, Writing, etc.)
3. Each subject drills into subcategories (Elementary, Secondary, ELL, Struggling Readers, Technology)
4. Individual program pages show: study count, effect sizes, evidence strength, program description

## Quick Reference

| Resource | URL |
|---|---|
| BEE Home | `https://bestevidence.org` |
| Reading / Elementary | `https://bestevidence.org/reading/elementary/` |
| Math / Elementary | `https://bestevidence.org/math/elementary/` |
| Math / Middle-High | `https://bestevidence.org/math/middle-high/` |
| Science / Elementary | `https://bestevidence.org/science/elementary/` |
| Writing / Grades 2-12 | `https://bestevidence.org/writing/` |
| Early Childhood | `https://bestevidence.org/early-childhood/` |
| COVID Learning Loss | `https://bestevidence.org/covid/` |
| Mental Health | `https://bestevidence.org/mental-health/` |
| Evidence for ESSA | `https://evidenceforessa.org` |
| Proven Tutoring | `https://proventutoring.org` |

### Subject Categories

| Subject | Subcategories |
|---|---|
| Reading | Elementary, Secondary, English Language Learners, Struggling Readers, Technology Effectiveness |
| Mathematics | Elementary, Middle/High School, Technology Effectiveness |
| Writing | Grades 2-12 |
| Science | Elementary, Secondary |
| Comprehensive School Reform | K-12 Meta-Analysis, Success for All |
| Early Childhood | Early Childhood Education |
| Special Populations | Special/Remedial Education, Summer School, COVID Learning Loss, Mental Health |

## Procedure

### 1. Find Programs by Subject

Navigate to `https://bestevidence.org` and expand a program review category. Each program listing includes:
- **Number of studies** reviewed
- **Effect size** (mean ES across studies)
- **Evidence strength** rating
- **Program description** and cost

### 2. Check ESSA Evidence Alignment

For ESSA Title I–IV compliance, cross-reference BEE ratings with the sister site:
- Navigate `browser_navigate` to `https://evidenceforessa.org`
- Search for a specific program name
- Returns ESSA evidence level (Strong, Moderate, Promising, Demonstrates Rationale)

### 3. Find Effect Sizes for Grant Applications

BEE reports include Cohen's d effect sizes which are required for many federal/state grant applications:

```
"[program] BEE effect size" site:bestevidence.org
```

### 4. Access Robert Slavin's Blog

Navigate to `https://bestevidence.org` → "Robert Slavin's Blog Archive" for decades of evidence-based reform commentary from the late CRRE director.

## Pitfalls

- **Not all programs reviewed** — BEE focuses on programs with multiple rigorous studies
- **Effect sizes vary by outcome** — Check individual study details, not just the mean
- **Sister sites are separate** — Evidence for ESSA and ProvenTutoring have different review criteria
- **WordPress platform** — Some pages require JavaScript; use `browser_navigate` not `web_extract`
- **Updates are periodic** — Reviews updated when new studies meet inclusion criteria

## Verification

```bash
curl -sk -o /dev/null -w '%{http_code}' https://bestevidence.org
# Expect: 200
```