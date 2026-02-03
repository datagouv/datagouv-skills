# data.gouv.fr Main API Reference

Complete API reference for the main data.gouv.fr API.

## Base URL

```
https://www.data.gouv.fr/api/1/
```

## Authentication

Most endpoints are public and don't require authentication. For write operations (POST, PUT, DELETE), include your API key:

```
X-API-KEY: your-api-key
```

Get your API key from: https://www.data.gouv.fr/fr/admin/me/

## Endpoints

### Datasets

#### List Datasets
```
GET /datasets/
```

**Query Parameters:**
- `q` - Search query
- `page` - Page number (default: 1)
- `page_size` - Items per page (default: 20, max: 100)
- `sort` - Sort field (e.g., `-created`, `title`)
- `organization` - Filter by organization ID
- `tag` - Filter by tag

**Example:**
```python
import requests
response = requests.get(
    "https://www.data.gouv.fr/api/1/datasets/",
    params={"q": "transport", "page_size": 10}
)
datasets = response.json()
```

#### Get Dataset
```
GET /datasets/{id}/
```

**Example:**
```python
dataset_id = "53699528a3a729239d2051c0"
response = requests.get(f"https://www.data.gouv.fr/api/1/datasets/{dataset_id}/")
dataset = response.json()
```

#### List Dataset Resources
```
GET /datasets/{id}/resources/
```

**Example:**
```python
response = requests.get(f"https://www.data.gouv.fr/api/1/datasets/{dataset_id}/resources/")
resources = response.json()
```

### Organizations

#### List Organizations
```
GET /organizations/
```

**Query Parameters:**
- `q` - Search query
- `page` - Page number
- `page_size` - Items per page

#### Get Organization
```
GET /organizations/{id}/
```

### Users

#### List Users
```
GET /users/
```

#### Get User
```
GET /users/{id}/
```

### Reuses

#### List Reuses
```
GET /reuses/
```

#### Get Reuse
```
GET /reuses/{id}/
```

## Response Format

All endpoints return JSON. Successful responses include:
- `data` - Array of results (for list endpoints)
- `page` - Current page number
- `page_size` - Items per page
- `total` - Total number of items

## Error Handling

- `200 OK` - Success
- `400 Bad Request` - Invalid parameters
- `404 Not Found` - Resource not found
- `401 Unauthorized` - Authentication required
- `403 Forbidden` - Insufficient permissions

## Rate Limiting

The API has rate limits. Check response headers:
- `X-RateLimit-Limit` - Request limit per hour
- `X-RateLimit-Remaining` - Remaining requests
- `X-RateLimit-Reset` - Reset time (Unix timestamp)

## Additional Resources

- Official API Documentation: https://guides.data.gouv.fr/guide-data.gouv.fr/readme-1/reference/
- Python Client: https://github.com/etalab/datagouv-client-python
