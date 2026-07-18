---
name: what-works-clearinghouse
description: "Search IES What Works Clearinghouse for evidence-based research."
version: 0.1.0
author: Hermes
metadata:
  hermes:
    tags: [IES, WWC, Research, Evidence, Education]
---

# What Works Clearinghouse (WWC)

The Institute of Education Sciences (IES) What Works Clearinghouse reviews and rates education research, interventions, and practices. Provides Practice Guides, Intervention Reports, and Individual Study Reviews with evidence tiers. Covers PK-12 and postsecondary topics with search by keyword, topic, and grade band. Data available at `https://ies.ed.gov/ncee/wwc`.

Does NOT provide raw API access — data is accessed through the web search interface and downloadable reports. No API key required for public access.

## When to Use

- "Is there evidence that [intervention] works?"
- "What does the research say about [topic]?"
- "Find WWC practice guides for literacy"
- "Check evidence tier for [program]"
- "What are ESSA evidence levels for [curriculum]?"

## Prerequisites

- None — public federal resource, no authentication required
- Use `web_search` with `site:ies.ed.gov/ncee/wwc` for targeted results
- Use `browser_navigate` to interact with the search interface at `https://ies.ed.gov/ncee/wwc`

## How to Run

1. Use `web_search` for quick lookups: `"[topic] site:ies.ed.gov/ncee/wwc"` or `"[intervention] WWC evidence"`
2. Use `browser_navigate` to `https://ies.ed.gov/ncee/wwc` for the full search interface
3. For specific topic searches, append `?topic=[topic]` parameters to filter results

## Quick Reference

| Resource | URL |
|---|---|
| Home / Search | `https://ies.ed.gov/ncee/wwc` |
| Practice Guides | `https://ies.ed.gov/ncee/wwc/PracticeGuides` |
| Intervention Reports | `https://ies.ed.gov/ncee/wwc/InterventionReports` |
| Individual Studies | `https://ies.ed.gov/ncee/wwc/StudyReviews` |
| Data Downloads | `https://ies.ed.gov/ncee/wwc/Data` |
| Resources for Educators | `https://ies.ed.gov/ncee/wwc/Resources` |
| Handbooks & Training | `https://ies.ed.gov/ncee/wwc/Handbooks` |

### Topics (Filter Categories)

Literacy, STEM, English Learners, Social Emotional Learning and Behavior, Children and Youth with Disabilities, Career and Technical Education, College Readiness and Completion, High School Completion, Out-of-School Learning, School Choice, Targeted and Intensive Interventions, Teachers and School Leaders

### Grade Bands

Preschool (PK), Elementary (K-5), Middle School (6-8), High School (9-12), Postsecondary (PS)

## Procedure

### 1. Search for an Intervention

Use `web_search` with `site:ies.ed.gov/ncee/wwc` for targeted results:

```
"[program name] WWC intervention report"
```

or use `browser_navigate` to the WWC home page and search by intervention name.

### 2. Find Practice Guides

Practice guides are the most actionable resource — they synthesize evidence into classroom-ready recommendations:

```
"[topic] WWC practice guide K-5 site:ies.ed.gov/ncee/wwc"
```

### 3. Check Evidence Tiers (ESSA Alignment)

WWC evidence tiers map to ESSA levels:
- **Tier 1 (Strong)** — Well-designed RCT with positive effects
- **Tier 2 (Moderate)** — Quasi-experimental with positive effects
- **Tier 3 (Promising)** — Correlational with statistical controls
- **Meets WWC Standards Without Reservations** — Highest quality rating

Search: `"[intervention] WWC evidence tier ESSA"`

### 4. Download Data

Navigate to `https://ies.ed.gov/ncee/wwc/Data` for downloadable datasets of study reviews, intervention ratings, and outcome domains in CSV/Excel format.

## Pitfalls

- **No public REST API** — Data is accessed through web search and downloadable files, not programmatic endpoints
- **Not all interventions reviewed** — Search may return no results for newer programs
- **Evidence tiers ≠ effectiveness** — "Meets Standards" means the study was well-designed, not that the intervention is definitely effective
- **Site uses JavaScript** — Some pages require `browser_navigate` rather than `web_extract`
- **Updates are periodic** — New reviews published monthly; check "What's New" section for latest

## Verification

```bash
# Verify the WWC site is accessible
curl -sk -o /dev/null -w '%{http_code}' https://ies.ed.gov/ncee/wwc
# Expect: 200
```