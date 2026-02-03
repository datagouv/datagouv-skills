# data.gouv.fr Main API Examples

Practical examples for using the main data.gouv.fr API.

## Example 1: Search for Transportation Datasets

```python
import requests

def search_transport_datasets():
    url = "https://www.data.gouv.fr/api/1/datasets/"
    params = {
        "q": "transport",
        "page_size": 20,
        "sort": "-created"
    }
    response = requests.get(url, params=params)
    data = response.json()
    
    print(f"Found {data['total']} datasets")
    for dataset in data['data']:
        print(f"- {dataset['title']} ({dataset['id']})")
    
    return data

# Usage
datasets = search_transport_datasets()
```

## Example 2: Get Dataset and Download Resources

```python
import requests

def get_dataset_resources(dataset_id):
    # Get dataset details
    dataset_url = f"https://www.data.gouv.fr/api/1/datasets/{dataset_id}/"
    dataset = requests.get(dataset_url).json()
    
    print(f"Dataset: {dataset['title']}")
    print(f"Description: {dataset['description'][:100]}...")
    
    # Get resources
    resources_url = f"https://www.data.gouv.fr/api/1/datasets/{dataset_id}/resources/"
    resources = requests.get(resources_url).json()
    
    print(f"\nResources ({len(resources['data'])}):")
    for resource in resources['data']:
        print(f"- {resource['title']}: {resource['url']}")
    
    return dataset, resources

# Usage
dataset_id = "53699528a3a729239d2051c0"
dataset, resources = get_dataset_resources(dataset_id)
```

## Example 3: Find Datasets by Organization

```python
import requests

def get_organization_datasets(org_id):
    url = "https://www.data.gouv.fr/api/1/datasets/"
    params = {
        "organization": org_id,
        "page_size": 100
    }
    response = requests.get(url, params=params)
    data = response.json()
    
    print(f"Organization has {data['total']} datasets")
    return data['data']

# Usage
org_id = "534fff75a3a7292c64a77de4"  # Example: INSEE
datasets = get_organization_datasets(org_id)
```

## Example 4: Search with Pagination

```python
import requests
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
        response = requests.get(url, params=params)
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
import requests

def get_datasets_by_tag(tag: str):
    url = "https://www.data.gouv.fr/api/1/datasets/"
    params = {
        "tag": tag,
        "page_size": 50
    }
    response = requests.get(url, params=params)
    data = response.json()
    
    print(f"Found {data['total']} datasets with tag '{tag}'")
    return data['data']

# Usage
geodata = get_datasets_by_tag("geodata")
```

## Example 6: Get Organization Details

```python
import requests

def get_organization_info(org_id: str):
    url = f"https://www.data.gouv.fr/api/1/organizations/{org_id}/"
    org = requests.get(url).json()
    
    print(f"Organization: {org['name']}")
    print(f"Description: {org.get('description', 'N/A')}")
    print(f"Website: {org.get('url', 'N/A')}")
    print(f"Datasets: {org.get('metrics', {}).get('datasets', 0)}")
    
    return org

# Usage
org_id = "534fff75a3a7292c64a77de4"
org = get_organization_info(org_id)
```
