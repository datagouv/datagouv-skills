# Metrics API — Reference

**Production:** `https://metric-api.data.gouv.fr`  
**Swagger (reference):** https://metric-api.data.gouv.fr/api/doc — use for up-to-date paths and parameters.

## Endpoints

- **`GET /api/{model}/data/`** — Paginated metrics for the given model (JSON). Response: `data` (array), `links` (next/prev), `meta` (page, page_size, total). Filter with `column__exact=value`, sort with `column__sort=asc` or `column__sort=desc`. 400 invalid query string, 404 model not found.
- **`GET /api/{model}/data/csv/`** — Same metrics as CSV. Filter/sort params apply; **pagination is not applied** (full result set is streamed). 400, 404.
- **`GET /health/`** — Health check. 200: JSON with status, version, uptime_seconds. 503: database unavailable.

**`{model}`** — Metrics table name (e.g. `site`, `organization`, `dataset`).

## Query parameters

- **JSON endpoint only:** `page` (1-based, default 1), `page_size`.
- **Both JSON and CSV:** `column__sort=asc` | `column__sort=desc`, `column__exact=value`, `column__contains=value`, `column__less=value`, `column__greater=value`.

Use the actual column name in place of `column`. Check the Swagger for column names per model.
