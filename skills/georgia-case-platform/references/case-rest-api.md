# CASE REST/JSON Binding v1.1 Endpoints

Base URL: `https://gadoe.georgiacase.org/ims/case/v1p1/`

All endpoints return JSON. Read access requires no authentication.

## Endpoint Details

### GET /CFDocuments
List all competency frameworks. Supports `limit`, `offset`, `sort`, `orderBy`, `filter`.

**Example response:**
```json
{
  "CFDocuments": [{
    "uri": "https://case.georgiastandards.org/ims/case/v1p1/CFDocuments/00fcf0e2-b9c3-11e7-a4ad-47f36833e889",
    "identifier": "00fcf0e2-b9c3-11e7-a4ad-47f36833e889",
    "lastChangeDateTime": "2025-06-30T23:05:56+00:00",
    "creator": "Georgia Department of Education",
    "title": "Computer Science - Georgia Standards of Excellence",
    "adoptionStatus": "Adopted"
  }]
}
```

### GET /CFDocuments/{sourcedId}
Returns a single CFDocument with nested CFItems, CFSubjects, etc.

### GET /CFItems
List individual competency/standard items. Returns `CFItems` array.

### GET /CFItemAssociations/{sourcedId}
Returns associations (alignments) between CASE items.

## Query Parameters
- `limit` (PositiveInteger) — max records per page
- `offset` (NonNegativeInteger, default 0) — pagination offset
- `sort` (String) — field name to sort by
- `orderBy` (Enum: asc|desc) — sort direction
- `filter` (String) — filter expression

## HTTP Codes
- 200 — Success
- 400 — Bad request
- 404 — SourcedId not found
- 500 — Internal server error

## Network Access
The service is behind GaDOE FortiGuard. Access from docker1:
```bash
ssh docker1 "curl -sk 'https://gadoe.georgiacase.org/ims/case/v1p1/CFDocuments?limit=3'"
```

## Related
- CASE Dashboard: `edtechlabs.dev/case/` — framework usage analytics
- CASE2 Neo4j: `case2.edtechlabs.dev:7474` — knowledge graph with framework GUIDs
- Transcript Alignments: `transcript.edtechlabs.dev` — course→CASE item resolution
