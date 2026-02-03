---
name: datagouv-main-api
description: Interact with the main data.gouv.fr API to search datasets, organizations, users, and resources. Use when working with data.gouv.fr datasets, organizations, users, reuses, discussions, or core platform features.
---

# data.gouv.fr Main API

Interact with the main data.gouv.fr API to access datasets, organizations, users, and other core resources.

**If the data.gouv.fr MCP server is configured,** prefer its tools for these operations when available. The details below (endpoints, parameters) still apply and can be used with or without MCP.

## Base URL

- **Production:** `https://www.data.gouv.fr/api/1/`
- **Demo (tests):** `https://demo.data.gouv.fr/api/1/`

## Auth, IDs, permissions

- **Write ops** (POST, PUT, PATCH, DELETE): require header `X-API-KEY` (API key from profile settings). Read ops are public.
- **URL IDs:** use technical id (e.g. `5bbb6d6cff66bd4dc17bfd5a`) or slug (e.g. `mon-dataset`). Prefer technical id for durable scripts (slug can change).
- **Permissions:** same as web (e.g. must be org member to edit an org’s dataset). Datasets created via API are **public** unless `private: true` (draft).
- **Content:** JSON in/out (`application/json`); file upload endpoints accept `multipart/form-data`.

## Common Endpoints

### Datasets
- `GET /datasets/` - List datasets
- `GET /datasets/{id}/` - Get dataset details
- `GET /datasets/{id}/resources/` - List dataset resources

### Organizations
- `GET /organizations/` - List organizations
- `GET /organizations/{id}/` - Get organization details

### Users
- `GET /users/` - List users
- `GET /users/{id}/` - Get user details

### Reuses
- `GET /reuses/` - List reuses
- `GET /reuses/{id}/` - Get reuse details

## Common Patterns

### Searching Datasets

```python
import requests

def search_datasets(query, page=1, page_size=20):
    url = "https://www.data.gouv.fr/api/1/datasets/"
    params = {
        "q": query,
        "page": page,
        "page_size": page_size
    }
    response = requests.get(url, params=params)
    return response.json()
```

### Getting Dataset Resources

```python
def get_dataset_resources(dataset_id):
    url = f"https://www.data.gouv.fr/api/1/datasets/{dataset_id}/resources/"
    response = requests.get(url)
    return response.json()
```

**Python:** For code in Python, an [official client](https://github.com/etalab/datagouv-client-python) is available; use it when appropriate instead of raw HTTP calls. The endpoints above still apply.

Pagination: list responses include `data`, `page`, `page_size`, `total`, `next_page`, `previous_page`. Errors: 400, 401, 403, 404, 423 (spam/suspicious — creation disabled), 500, 502. Body may include `{"message": "..."}`. See [reference.md](reference.md) for full details.
