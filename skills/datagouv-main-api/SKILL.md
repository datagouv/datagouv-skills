---
name: datagouv-main-api
description: Interact with the main data.gouv.fr API to search datasets, organizations, users, and resources. Use when working with data.gouv.fr datasets, organizations, users, reuses, discussions, or core platform features.
---

# data.gouv.fr Main API

Interact with the main data.gouv.fr API to access datasets, organizations, users, and other core resources.

## Base URL

```
https://www.data.gouv.fr/api/1/
```

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

## Authentication

For write operations, include an API key in the `X-API-KEY` header:
```
X-API-KEY: your-api-key
```

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

## Additional Resources

- For complete API reference, see [reference.md](reference.md)
- For usage examples, see [examples.md](examples.md)
