# Tabular API — Reference

**Production:** `https://tabular-api.data.gouv.fr`  
**Swagger (reference):** https://tabular-api.data.gouv.fr/api/doc  
**Per-resource OpenAPI:** `GET /api/resources/{rid}/swagger/` — data endpoint, columns and filter/sort operators. Swagger is the source of truth.

## Endpoints

- `GET /api/resources/{rid}/` — Resource metadata (created_at, url, links with rel: profile, data, swagger).
- `GET /api/resources/{rid}/profile/` — Resource profile: column types, formats, stats (encoding, separator, total_lines, etc.). Response includes `profile` (csv_detective) and `indexes` (column names that have an index, for filtering/sorting); nullable if none.
- `GET /api/resources/{rid}/swagger/` — OpenAPI spec for this resource’s data and CSV endpoints (available columns and operators).
- `GET /api/aggregation-exceptions/` — List of resource UUIDs for which aggregation (groupby, count, sum, etc.) is allowed.
- `GET /api/resources/{rid}/data/`, `.../data/csv/`, `.../data/json/` — Data (spec in resource’s swagger).
- `GET /health/` — Health check. 200: JSON with status, version, uptime_seconds. 503: database unavailable.

## Data endpoint: query operators

Replace `column_name` with the actual column name. Allowed operators depend on column type (see resource’s swagger).

**Filtering:** `column__exact=value`, `column__differs=value`, `column__isnull`, `column__isnotnull`, `column__contains=value`, `column__notcontains=value`, `column__in=v1,v2,v3`, `column__notin=v1,v2,v3`, `column__less=value`, `column__greater=value`, `column__strictly_less=value`, `column__strictly_greater=value`.

**Sorting:** `column__sort=asc`, `column__sort=desc`.

**Aggregation** (only on resources in aggregation allow-list and on indexed columns; see `/api/aggregation-exceptions/`): `column__groupby`, `column__count`, `column__avg`, `column__min`, `column__max`, `column__sum`. Aggregation result keys are like `column__sum`, `column__avg`. Filtering is applied before aggregation.

**Pagination:** `page=1`, `page_size=20` (max 50).

**Column selection:** `columns=col1,col2,col3`.

**Important:** Columns that contain JSON objects do not support filtering or aggregation except `isnull` and `isnotnull`. Check the profile to see which columns are JSON.
