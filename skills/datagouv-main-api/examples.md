# data.gouv.fr Main API Examples

Practical examples for using the main data.gouv.fr API.

## Example 1: Search for Transportation Datasets

```python
import httpx

def search_transport_datasets():
    url = "https://www.data.gouv.fr/api/1/datasets/"
    params = {
        "q": "transport",
        "page_size": 20,
        "sort": "-created"
    }
    response = httpx.get(url, params=params)
    data = response.json()
    
    print(f"Found {data['total']} datasets")
    for dataset in data['data']:
        print(f"- {dataset['title']} ({dataset['id']})")
    
    return data

# Usage
datasets = search_transport_datasets()
```

## Example 2: Get Dataset and Its Resources

Resources are embedded in the dataset object (no separate `GET /resources/` endpoint).

```python
import httpx

def get_dataset_resources(dataset_id: str):
    dataset_url = f"https://www.data.gouv.fr/api/1/datasets/{dataset_id}/"
    response = httpx.get(dataset_url)
    response.raise_for_status()
    dataset = response.json()
    
    print(f"Dataset: {dataset['title']}")
    print(f"Description: {dataset['description'][:100]}...")
    
    # Resources are in dataset['resources']
    resources = dataset.get("resources", [])
    print(f"\nResources ({len(resources)}):")
    for resource in resources:
        print(f"- {resource['title']}: {resource['url']}")
    
    return dataset

# Usage (use a dataset ID from search results)
dataset_id = "53ba5b91a3a729219b7beae9"  # Example dataset
dataset = get_dataset_resources(dataset_id)
```

## Example 3: Find Datasets by Organization

```python
import httpx

def get_organization_datasets(org_id: str):
    url = "https://www.data.gouv.fr/api/1/datasets/"
    params = {
        "organization": org_id,
        "page_size": 100
    }
    response = httpx.get(url, params=params)
    data = response.json()
    
    print(f"Organization has {data['total']} datasets")
    return data['data']

# Usage
org_id = "534fff75a3a7292c64a77de4"  # Example: INSEE
datasets = get_organization_datasets(org_id)
```

## Example 4: Search with Pagination

```python
import httpx
from typing import List, Dict

def get_all_datasets(query: str, max_results: int = 100) -> List[Dict]:
    """Fetch all datasets matching a query with pagination."""
    all_datasets = []
    page = 1
    page_size = 100
    
    while len(all_datasets) < max_results:
        url = "https://www.data.gouv.fr/api/1/datasets/"
        params = {
            "q": query,
            "page": page,
            "page_size": page_size
        }
        response = httpx.get(url, params=params)
        data = response.json()
        
        if not data['data']:
            break
        
        all_datasets.extend(data['data'])
        
        # Check if we've reached the end
        if len(all_datasets) >= data['total']:
            break
        
        page += 1
    
    return all_datasets[:max_results]

# Usage
all_transport = get_all_datasets("transport", max_results=200)
print(f"Retrieved {len(all_transport)} datasets")
```

## Example 5: Filter by Tags

```python
import httpx

def get_datasets_by_tag(tag: str):
    url = "https://www.data.gouv.fr/api/1/datasets/"
    params = {
        "tag": tag,
        "page_size": 50
    }
    response = httpx.get(url, params=params)
    data = response.json()
    
    print(f"Found {data['total']} datasets with tag '{tag}'")
    return data['data']

# Usage (e.g. transport, donnees-ouvertes, amenagements-cyclables)
datasets = get_datasets_by_tag("transport")
```

## Example 6: Get Organization Details

```python
import httpx

def get_organization_info(org_id: str):
    url = f"https://www.data.gouv.fr/api/1/organizations/{org_id}/"
    response = httpx.get(url)
    response.raise_for_status()
    org = response.json()
    
    print(f"Organization: {org['name']}")
    print(f"Description: {org.get('description', 'N/A')}")
    print(f"Website: {org.get('url', 'N/A')}")
    print(f"Datasets: {org.get('metrics', {}).get('datasets', 0)}")
    
    return org

# Usage
org_id = "534fff75a3a7292c64a77de4"
org = get_organization_info(org_id)
```
