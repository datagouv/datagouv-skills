---
name: datagouv-tabular-api
description: Query tabular/CSV data for data.gouv.fr resources by resource ID. Use when working with tabular data, CSV resources, filtering/sorting resource rows, or resource profile (column types). Requires a resource ID from the main data.gouv.fr API.
---

# data.gouv.fr Tabular API

Query tabular data for a given **resource ID** (from the main data.gouv.fr API). Supports filtering, sorting, pagination, and aggregation.

**If the data.gouv.fr MCP server is configured,** prefer its tools for Tabular API operations when available. The details below (endpoints, operators) still apply with or without MCP.

**Production base URL:** `https://tabular-api.data.gouv.fr`  
**Authoritative reference (Swagger):** https://tabular-api.data.gouv.fr/api/doc — **check the Swagger for up-to-date endpoints and parameters.**

## Endpoints

The main Swagger lists metadata, profile, and the per-resource OpenAPI spec. The **data** endpoints (`/data/`, `/data/csv/`, `/data/json/`) are described in each resource’s own spec (see `GET .../swagger/`).

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/resources/{rid}/` | Resource metadata (created_at, url, links to profile, data, swagger) |
| GET | `/api/resources/{rid}/profile/` | Resource profile: column types, formats, stats (encoding, separator, row count); includes `indexes` (columns with an index) |
| GET | `/api/resources/{rid}/swagger/` | OpenAPI spec for this resource’s **data** endpoint (columns and filter/sort operators) |
| GET | `/api/aggregation-exceptions/` | List of resource UUIDs for which aggregation (groupby, count, sum, etc.) is allowed |
| GET | `/api/resources/{rid}/data/` | Data with filtering, sorting, pagination (spec in resource’s swagger) |
| GET | `/api/resources/{rid}/data/csv/` | Stream data as CSV |
| GET | `/api/resources/{rid}/data/json/` | Stream data as JSON |
| GET | `/health/` | Health check (200 + status/version/uptime; 503 if DB unavailable) |

**Note:** `{rid}` is the data.gouv.fr resource **UUID** (from the main API). Get it from a dataset’s resources, then use it here.

## Query parameters (data endpoint)

- **Pagination:** `page=1`, `page_size=20` (max 50).
- **Columns:** `columns=col1,col2,col3` to return only those columns.
- **Filtering** (use column name; check resource’s swagger for allowed ops): `column__exact=value`, `column__differs=value`, `column__isnull`, `column__isnotnull`, `column__contains=value`, `column__notcontains=value`, `column__in=a,b,c`, `column__notin=a,b,c`, `column__less=value`, `column__greater=value`, `column__strictly_less=value`, `column__strictly_greater=value`.
- **Sorting:** `column__sort=asc` or `column__sort=desc`.
- **Aggregation** (only on allowed resources and indexed columns; see `/api/aggregation-exceptions/`): `column__groupby`, `column__count`, `column__avg`, `column__min`, `column__max`, `column__sum`. Result keys look like `column__sum`, `column__avg`, etc. **JSON columns** only support `isnull`/`isnotnull`.

Always use the **resource’s own OpenAPI spec** (`GET /api/resources/{rid}/swagger/`) for the data endpoint’s exact parameters and allowed operators per column.

## Example: get metadata and first page of data

```python
import requests

BASE = "https://tabular-api.data.gouv.fr"

def tabular_meta(resource_id: str):
    r = requests.get(f"{BASE}/api/resources/{resource_id}/")
    return r.json()

def tabular_data(resource_id: str, page=1, page_size=20, **params):
    r = requests.get(f"{BASE}/api/resources/{resource_id}/data/", params={"page": page, "page_size": page_size, **params})
    return r.json()
```

## Example: filter and sort

```bash
# Filter: score > 0.9, decompte = 13
curl "https://tabular-api.data.gouv.fr/api/resources/{resource_id}/data/?score__greater=0.9&decompte__exact=13"

# Pagination
curl "https://tabular-api.data.gouv.fr/api/resources/{resource_id}/data/?page=2&page_size=30"

# Select columns
curl "https://tabular-api.data.gouv.fr/api/resources/{resource_id}/data/?columns=id,score,birth"
```

For full operator list, aggregation rules, and JSON column limits, see [reference.md](reference.md). **When in doubt, use the Swagger:** https://tabular-api.data.gouv.fr/api/doc and the per-resource `GET /api/resources/{rid}/swagger/`.
