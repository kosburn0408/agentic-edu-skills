---
name: transcript-alignments
description: "Parse HS transcripts and resolve CASE competency alignments."
version: 0.1.0
author: Hermes
metadata:
  hermes:
    tags: [GaDOE, CASE, Transcripts, LER, Competencies, Education]
---

# Georgia High School Transcript Alignments

Upload high school transcript files (Xap standard fixed-width `.txt`/`.trs`) and generate **Learner Employment Records (LER)** — academic courses resolved against Georgia CASE competency frameworks. Runs on docker1 as a React/Vite app in an nginx container at **https://transcript.edtechlabs.dev** (port 3020).

This skill covers **deployment, maintenance, and troubleshooting**. It does NOT cover the CASE competency framework data itself — that lives in the CASE dashboard and SuitCASE API.

## When to Use

- "Upload a transcript to the Transcript Alignments app."
- "Check the Transcript app on docker1."
- "Fix the transcript parsing for a course code."
- "Add demo data to the Transcript app."
- "Restart the transcript container."
- "Update the CASE lookup table in the transcript app."

## Architecture

| Component | Tech | Location |
|---|---|---|
| Frontend | React 19 + Vite | `/home/kosburn0408/transcript/app/` on docker1 |
| Container | nginx:alpine | `transcript-app` (port 3020) |
| CASE Lookup | Embedded JS object | Built into the React bundle |
| Demo Data | Bitsy & Riggs Osburn | Hardcoded Xap transcripts |

## Prerequisites

- SSH access to docker1: `ssh docker1` (user `kosburn0408`)
- Docker running on docker1
- Source code at `/home/kosburn0408/transcript/app/`

## How to Run

### Access the app
Open `https://transcript.edtechlabs.dev` in a browser, or use `browser_navigate`.

### Restart the container
```bash
ssh docker1 "docker restart transcript-app"
```

### Rebuild and redeploy
```bash
ssh docker1 "cd /home/kosburn0408/transcript/app && docker build -t transcript-app . && docker stop transcript-app && docker rm transcript-app && docker run -d --name transcript-app -p 3020:80 --restart unless-stopped transcript-app"
```

## Quick Reference

| Endpoint | Description |
|---|---|
| `https://transcript.edtechlabs.dev/` | Main app — upload transcript or load demos |
| `GET /` | React SPA (static files from nginx) |
| Docker port | `3020:80` (host:container) |
| Container name | `transcript-app` |
| Dockerfile | `/home/kosburn0408/transcript/app/Dockerfile` |

## Procedure

### 1. Parse a Transcript

The app parses **Xap standard fixed-width format** with ASCII Record Separators (`\x1e`):

| Record Type | Contains |
|---|---|
| `HR` | Header — Xap format version |
| `01` | School info (name, address, CEEB code) |
| `02` | Student info (name, DOB, GPA, graduation year) |
| `03` | Session header (term, grade level, dates) |
| `04` | Course record (title, grade, credits, course number) |
| `05` | Test scores |
| `06` | Immunization records |

### 2. CASE Competency Resolution

The app matches each course's **course number** against a lookup table mapping:
```
course_number → [{title, framework_title, framework_id, item_id}]
```

Course numbers are matched with multiple strategies:
- **Exact match** — full course number
- **Prefix match** — first 8 characters
- **Normalized match** — handling leading zeros after decimal
- **Subject fallback** — maps subject area prefix to default framework

### 3. Use Demo Data

Click **"Load Demo Transcripts (Bitsy & Riggs)"** to see the LER output. The demo data contains complete 4-year transcripts for the Osburn papillons, showing:
- All courses taken across 8 semesters
- Grades, credits, GPA
- CASE competency alignments per course
- Framework coverage summary

### 4. Add New Course Mappings

The lookup tables live in the built JS bundle at:
```
/usr/share/nginx/html/assets/index-*.js
```

To add new course mappings, edit the source at `/home/kosburn0408/transcript/app/src/` and rebuild.

## Pitfalls

- **The CASE lookup table is embedded in the JS bundle** — not a live API. Changes to CASE frameworks require rebuilding the container.
- **Only Xap format supported** — the parser expects `\x1e` (ASCII 0x1E) record separators, not CSV or JSON.
- **Demo data is hardcoded** — Bitsy and Riggs have fictional high school careers at "Jeff Davis High School." Their course numbers are real Georgia course codes.
- **Container has no volume mounts** — all customizations require rebuilding the Docker image.
- **Course number lookup is best-effort** — unknown course numbers fall back to subject area matching. Some courses may show "Unknown Course."

## Verification

```bash
# Check the app is serving
curl -sk -o /dev/null -w '%{http_code}' https://transcript.edtechlabs.dev
# Expected: 200

# Check the container is running
ssh docker1 "docker ps --format '{{.Status}}' --filter name=transcript-app"
# Expected: Up <time>
```
