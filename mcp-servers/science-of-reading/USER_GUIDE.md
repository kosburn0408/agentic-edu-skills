# Science of Reading MCP Server — User Guide

## What Is This?

A tool that analyzes any text through the lens of the **Science of Reading** — the body of research on how children learn to read. Teachers upload a passage and instantly get:

- **Lexile score** — how hard is this text?
- **Decodability** — what percentage of words can a K-3 student sound out?
- **Vocabulary tiers** — which words are everyday, academic, or domain-specific?
- **Evidence alignment** — does this approach match what research says works?

It runs as a Docker container on your computer. No cloud, no subscription, no data leaving your machine.

---

## Quick Start (5 Minutes)

### Step 1: Install Docker

Mac: `brew install --cask docker`
Windows: [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

### Step 2: Download the Server

```bash
git clone https://github.com/kosburn0408/agentic-edu-skills.git
cd agentic-edu-skills/mcp-servers/science-of-reading
```

### Step 3: Start It

```bash
docker compose up -d
```

Wait 30 seconds. The server is now running.

---

## Using the Server

### Option A: With Hermes Agent (AI Assistant)

If you use Hermes Agent, the server auto-connects. Just ask:

> *"Analyze this text for Lexile level and vocabulary tiers"*
>
> *"Is this passage decodable for a first grader?"*
>
> *"What does the research say about phonics instruction?"*

### Option B: Direct via Python

```python
from tools.lexile import compute_lexile
from tools.vocabulary import classify_text
from tools.decodability import check_decodability

# Your text
text = """Hooch sniffed the morning air. His tail gave one sharp wag.
Something waited around the river bend. The trees whispered secrets."""

# Analyze
lexile = compute_lexile(text)
vocab = classify_text(text)
decode = check_decodability(text, grade=2)

print(f"Lexile: {lexile['lexile_score']}L (Grade {lexile['grade_level']})")
print(f"Decodable: {decode['decodable_pct']}%")
print(f"Tier 2 words: {[w['word'] for w in vocab['word_details'] if w['tier'] == 2]}")
```

### Option C: One-Shot Seed (Populate the Database)

```bash
docker compose --profile seed up
```

This pre-loads the database with research papers, vocabulary lists, and standards. One-time only.

---

## What Each Tool Does

### 📊 Lexile Analysis (`compute_lexile`)

| Input | Output |
|---|---|
| Any English text | Lexile score (e.g., 660L) |
| | Grade level (e.g., "4") |
| | Word count, sentence length, rare word ratio |
| | Framework guidance (Simple View of Reading) |

**Example output:**
```
Lexile: 660L | Grade: 4
Framework: Simple View of Reading — Linguistic comprehension
increasingly drives reading outcomes as decoding becomes automatic.
```

### 🔤 Decodability Check (`check_decodability`)

| Input | Output |
|---|---|
| Text + target grade (K-3) | % of words that are decodable |
| | List of non-decodable words |
| | Grade-specific phonics patterns checked |

K checks CVC words (cat, dog). Grade 1 adds blends (stop, frog). Grade 2 adds digraphs (ship, chat). Grade 3 adds multisyllabic decoding.

### 📚 Vocabulary Tiers (`classify_text`)

Based on Beck, McKeown & Kucan's tiered vocabulary framework:

| Tier | Examples | What It Means |
|---|---|---|
| **Tier 1** | dog, river, tree, run | Everyday words — kids already know these |
| **Tier 2** | observe, investigate, protect | Academic words — teach these explicitly |
| **Tier 3** | tributary, ecosystem, conservation | Domain-specific — teach in context |

### 🔬 Evidence Search (`search_evidence`)

Query the embedded research database:

```python
from tools.evidence import search_evidence
results = search_evidence("phonics")
# Returns WWC Practice Guides, BEE meta-analyses, NRP findings
```

### 📋 Standards Alignment (`align_standards`)

Check if a text aligns with state standards:

```python
from tools.evidence import align_standards
standards = align_standards(grade="2", subject="ELA", state="Georgia")
```

---

## Real-World Examples

### Example 1: Evaluating a Book for Your Classroom

> *"Is Hooch the River Dog appropriate for my second graders?"*

```python
text = open("hooch-chapter1.txt").read()
r = compute_lexile(text)
print(f"Lexile: {r['lexile_score']}L → Grade {r['grade_level']}")
# → Lexile: 550L → Grade 2 ✅
```

### Example 2: Building a Vocabulary Lesson

> *"Which words should I pre-teach before reading Chapter 4?"*

```python
v = classify_text(chapter4_text)
tier2 = [w['word'] for w in v['word_details'] if w['tier'] == 2]
print("Pre-teach these words:", tier2)
# → ['debris', 'observation', 'current']
```

### Example 3: Checking Decodability for Early Readers

> *"Can my struggling first graders read this independently?"*

```python
d = check_decodability(text, grade=1)
print(f"{d['decodable_pct']}% decodable")
if d['decodable_pct'] < 80:
    print("⚠️ May need support for:", d['non_decodable'])
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "Docker not found" | Install Docker Desktop |
| "Connection refused" | Wait 30s for startup, then retry |
| "DB not seeded" | Run `docker compose --profile seed up` |
| Empty results | Check text has at least 5 words |
| "Port already in use" | Change port in docker-compose.yml |

---

## Theoretical Frameworks Embedded

Every tool response includes framework annotations. The server references:

| Framework | What It Says |
|---|---|
| **Simple View of Reading** (Gough & Tunmer 1986) | Reading = Decoding × Language Comprehension |
| **Scarborough's Rope** (2001) | Reading weaves word recognition + language comprehension strands |
| **Five Pillars** (NRP 2000) | Phonemic Awareness, Phonics, Fluency, Vocabulary, Comprehension |
| **WWC Practice Guides** | Evidence-based recommendations from IES What Works Clearinghouse |
| **BEE Meta-Analyses** | Effect sizes from Johns Hopkins Best Evidence Encyclopedia |

---

## Next Steps

1. **Try it with Hooch** — run Lexile analysis on a chapter
2. **Pre-teach vocabulary** — generate Tier 2 word lists for each chapter
3. **Align with Georgia standards** — check GSE alignment
4. **Share with colleagues** — the repo is MIT-licensed, free for any educator

## Support

Issues and feature requests: https://github.com/kosburn0408/agentic-edu-skills/issues