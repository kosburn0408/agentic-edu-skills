---
name: georgia-case-platform
description: "Query Georgia SuitCASE standards and CASE framework APIs."
version: 0.1.0
author: Hermes
metadata:
  hermes:
    tags: [GaDOE, CASE, Standards, SuitCASE, Georgia, Education]
related_skills: [case-fortireports-pipeline]
---

# Georgia SuitCASE — CASE Standards Platform

Access Georgia's competency frameworks via the SuitCASE Standards Satchel at `georgiacase.org`. Serves K-12 (GaDOE), technical college (TCSG), and higher education (Georgia Tech) standards in the machine-readable **CASE® format** (1EdTech standard). Runs on the Common Good Learning Tools Standards Satchel framework (Vue.js SPA).

This skill covers **framework browsing, standard lookup, and API integration**. It does NOT replace the CASE Dashboard (`edtechlabs.dev/case/`) which tracks framework usage analytics — that's a separate system.

## When to Use

- "Find the Georgia science standards for grade 2."
- "What CASE frameworks are available on SuitCASE?"
- "Look up standard SKL1 on georgiacase.org."
- "Check TCSG competencies for welding."
- "Get the CASE item GUID for Mathematics standard GSE 2.NBT.1."

## Prerequisites

- Access from the GaDOE network or docker1 (10.100.0.21) — FortiGuard blocks external IPs
- SSH to docker1: `ssh docker1`
- The site is a Vue.js SPA — JavaScript required, API calls go through the app layer

## How to Run

Query frameworks through the `browser_navigate` tool (JavaScript SPA) or proxy API calls through docker1 via the `terminal` tool:

```bash
ssh docker1 "curl -sk https://georgiacase.org/api/..."
```

## Quick Reference

| URL | Agency |
|---|---|
| `https://georgiacase.org` | Multi-agency portal / Standards Satchel home |
| `https://gadoe.georgiacase.org` | **GaDOE** — K-12 Georgia Standards of Excellence |
| `https://tcsg.georgiacase.org` | **TCSG** — Technical College System of Georgia |
| `https://gatech.georgiacase.org` | **Georgia Tech** — Higher education competencies |

| Config | Value |
|---|---|
| Framework | CASE® (1EdTech Competency and Academic Standards Exchange) |
| Primary color | `#1E4E55` (deep teal), accent `#CCA73B` (gold) |
| Font | Roboto |
| Embedded in | `inspire.gadoe.org`, `velocity.gadoe.org` |
| App name | Georgia SuitCASE / Standards Satchel |

## Key Framework Areas (GaDOE)

| Subject | CASE Framework ID Example |
|---|---|
| English Language Arts | `391c3abe-c1ec-4a4a-a942-c9e152b35102` |
| Mathematics | `e9dd7229-3558-4df2-85c6-57b8938f6180` |
| Science | `27a08dc6-416e-11e7-ba71-02bd89fdd987` |
| Social Studies | `a446e74c-463e-11e7-94f5-b49cee8b2d8c` |
| CTAE Career Clusters | Multiple — varies by cluster |
| Physical Education | `d79f5948-e166-11e8-8ec3-0242ac150004` |
| Health | `2f0b354a-43a8-11ea-a629-0242ac150004` |
| Fine Arts (Music) | `f3b94c72-9c0d-11e8-b85c-3b1a3079ae6e` |
| World Languages | `170214d7-fdcb-4886-8b72-4b370b5029b7` |

## Procedure

### 1. Browse frameworks

Use `browser_navigate` to open `https://gadoe.georgiacase.org`. The Standards Satchel SPA shows a framework browser with subject areas. Click through to drill into standards by grade level, subject, or keyword search.

### 2. Query a specific standard

Navigate to the framework, search for the standard code (e.g., "SKL1"), and the app returns the full CASE item including GUID, human-readable statement, and parent/child relationships.

### 3. Access from docker1

When external access is blocked by FortiGuard, proxy through docker1:
```bash
ssh docker1 "curl -sk 'https://gadoe.georgiacase.org/api/...' | python3 -m json.tool"
```

### 4. Cross-reference with CASE Dashboard

The CASE Dashboard at `edtechlabs.dev/case/` tracks which frameworks are being accessed and by which states. Use the SuitCASE portal to find standards, and the Dashboard to see usage analytics.

## Related Systems

| System | Connection |
|---|---|
| **CASE Dashboard** | Tracks 60 frameworks, 28.9M requests — analytics layer on top of SuitCASE |
| **FortiReports Pipeline** | Weekly email → CSV → ETL automation (skill: `case-fortireports-pipeline`) |
| **Transcript Alignments** | Resolves HS course codes → CASE items from SuitCASE |
| **CASE2 Neo4j** | Knowledge graph of states, frameworks, and relationships |
| **Preflight** | Board meeting tool that references CASE competency data |
| **1EdTech Registry** | The global CASE standard — `https://case.1edtech.org` |

## CASE REST API

The full 1EdTech CASE REST/JSON Binding v1.1 is implemented at the SuitCASE endpoints. The API requires no authentication for read access.

Base URL: `https://gadoe.georgiacase.org/ims/case/v1p1/`

### API Endpoints

| Service Call | Endpoint | Method | Description |
|---|---|---|---|
| `getAllCFDocuments` | `/CFDocuments` | GET | List all competency frameworks |
| `getCFDocument` | `/CFDocuments/{id}` | GET | Single framework with items |
| `getCFItem` | `/CFItems/{id}` | GET | Individual standard/competency |
| `getCFItemAssociations` | `/CFItemAssociations/{id}` | GET | Alignments between items |
| `getCFAssociation` | `/CFAssociations/{id}` | GET | Single association |
| `getCFSubject` | `/CFSubjects/{id}` | GET | Subject area |
| `getCFPackage` | `/CFPackages/{id}` | GET | Full framework package |
| `getCFRubric` | `/CFRubrics/{id}` | GET | Rubric definition |
| `getCFItemType` | `/CFItemTypes/{id}` | GET | Item type definition |

### Query Parameters

| Param | Example | Description |
|---|---|---|
| `limit` | `?limit=10` | Max records per page |
| `offset` | `?offset=20` | Skip N records |
| `sort` | `?sort=title` | Sort field |
| `orderBy` | `?orderBy=asc` | Sort direction (asc/desc) |
| `filter` | `?filter=title~Georgia` | Filter results |

### Live Examples (verify from docker1)

```bash
# List all 55 frameworks
ssh docker1 "curl -sk 'https://gadoe.georgiacase.org/ims/case/v1p1/CFDocuments?limit=3' | python3 -m json.tool"

# Get a specific framework
ssh docker1 "curl -sk 'https://gadoe.georgiacase.org/ims/case/v1p1/CFDocuments/00fcf0e2-b9c3-11e7-a4ad-47f36833e889' | python3 -m json.tool"

# Search for a standard
ssh docker1 "curl -sk 'https://gadoe.georgiacase.org/ims/case/v1p1/CFItems?limit=1' | python3 -m json.tool"
```

### Response Format

```json
{
  "CFDocuments": [{
    "uri": "https://case.georgiastandards.org/ims/case/v1p1/CFDocuments/...",
    "identifier": "GUID",
    "lastChangeDateTime": "2025-06-30T23:05:56+00:00",
    "creator": "Georgia Department of Education",
    "title": "Framework Name",
    "adoptionStatus": "Adopted"
  }]
}
```

Full REST binding spec: `references/case-rest-api.md`

## Pitfalls

- **FortiGuard blocks external IPs** — the site returns "Web Page Blocked" when accessed from outside the GaDOE network. Proxy ALL API calls through docker1 via SSH.
- **JavaScript required for browser UI** — the Vue.js SPA needs a browser. Raw `curl` returns the SPA shell from `/` — use `/ims/case/v1p1/` for the REST API.
- **The API is CASE REST/JSON Binding v1.1** — not a custom/internal API. It implements the 1EdTech standard at `/ims/case/v1p1/CFDocuments`, not at `/api/` or `/frameworks/`.
- **Framework IDs are stable GUIDs** — they don't change. The IDs in this skill's Quick Reference are verified against the CASE Dashboard data.
- **`getCFDocument` returns items inline** — for frameworks with many standards, use ?limit to paginate.
- **CASE Dashboard ETL runs from docker1 host, NOT the WordPress container** — the WordPress Docker image lacks Python. The `/var/www/html` directory is bind-mounted at `/home/kosburn0408/docker/wordpress/html/` on docker1, so run the ETL from docker1 directly. See `case-fortireports-pipeline` for the full pipeline.

## Verification

```bash
# Verify full pipeline: site reachable + API working
ssh docker1 "curl -sk -o /dev/null -w '%{http_code}' https://georgiacase.org && echo ' — site OK'"
ssh docker1 "curl -sk 'https://gadoe.georgiacase.org/ims/case/v1p1/CFDocuments?limit=1' | python3 -c 'import sys,json; d=json.load(sys.stdin); print(f\"Frameworks: {len(d.get(\"CFDocuments\",[]))} — API OK\")'"
```
