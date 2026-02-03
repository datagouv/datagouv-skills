---
name: datagouv-metrics-api
description: Retrieve metrics data for data.gouv.fr (by model type). Use when analyzing platform usage, download statistics, or traffic data. Supports filtering, sorting, and CSV export.
---

# data.gouv.fr Metrics API

Retrieve metric data for a given **model** (object type). Supports filtering, sorting, pagination, and CSV export.

**Production base URL:** `https://metric-api.data.gouv.fr`  
**Authoritative reference (Swagger):** https://metric-api.data.gouv.fr/api/doc — **check the Swagger for up-to-date endpoints and parameters.**

## Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/{model}/data/` | Metrics by model (JSON). Response: `data`, `links.next`/`links.prev`, `meta.page`/`page_size`/`total`. 400 invalid query, 404 model not found. |
| GET | `/api/{model}/data/csv/` | Same metrics as CSV stream. Filter/sort params apply; **pagination is not applied** (full result set streamed). |
| GET | `/health/` | Health check. 200: JSON with status, version, uptime_seconds. 503: database unavailable. |

**`{model}`** = metrics table name (e.g. `site`, `organization`, `dataset`). See Swagger for allowed values.

## Query parameters (from Swagger)

- **Pagination (JSON only):** `page` (1-based, default 1), `page_size`
- **Sorting:** `column__sort=asc`, `column__sort=desc`
- **Filtering:** `column__exact=value`, `column__contains=value`, `column__less=value`, `column__greater=value`

Replace `column` with the actual column name for the model. Check the Swagger for column names per model.

## Example

```python
import requests

BASE = "https://metric-api.data.gouv.fr"

def metrics_data(model: str, page=1, page_size=20, **params):
    r = requests.get(f"{BASE}/api/{model}/data/", params={"page": page, "page_size": page_size, **params})
    return r.json()

def metrics_csv(model: str, **params):
    r = requests.get(f"{BASE}/api/{model}/data/csv/", params=params)
    return r.text  # or stream
```

```bash
# JSON
curl "https://metric-api.data.gouv.fr/api/{model}/data/"

# CSV
curl "https://metric-api.data.gouv.fr/api/{model}/data/csv/"

# Health
curl "https://metric-api.data.gouv.fr/health/"
```

Replace `{model}` with the metrics table name (e.g. `site`, `organization`, `dataset`). **When in doubt, use the Swagger:** https://metric-api.data.gouv.fr/api/doc.
