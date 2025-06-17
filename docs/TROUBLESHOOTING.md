# Troubleshooting Guide

## Pinecone Metadata Blob Issue

### Problem Description
When inserting data into Pinecone from n8n workflow, the metadata is being stored as a single blob instead of individual fields.

### Expected Behavior
```json
{
  "id": "doc_123",
  "values": [0.1, 0.2, ...],
  "metadata": {
    "title": "Cognitive Behavioral Therapy Techniques",
    "source": "github.com/therapy-resources",
    "category": "therapy",
    "subcategory": "CBT",
    "date_scraped": "2025-06-16",
    "content_type": "markdown",
    "tags": ["mental-health", "therapy", "CBT"]
  }
}
```

### Actual Behavior
```json
{
  "id": "doc_123",
  "values": [0.1, 0.2, ...],
  "metadata": {
    "blob": "{\"title\":\"Cognitive Behavioral Therapy Techniques\",\"source\":\"github.com/therapy-resources\"...}"
  }
}
```

## Solutions

### Solution 1: Fix n8n Pinecone Node Configuration

In your n8n workflow, ensure the Pinecone node is configured correctly:

1. **Check the Metadata Field Mapping**
   - Open your n8n workflow
   - Go to the Pinecone node
   - In the metadata section, use "Add Field" for each metadata field
   - Don't use "JSON" mode, use "Fixed Collection" mode

2. **Example Configuration**:
   ```
   Metadata:
   - Field Name: title | Value: {{ $json.title }}
   - Field Name: source | Value: {{ $json.source }}
   - Field Name: category | Value: {{ $json.category }}
   - Field Name: date_scraped | Value: {{ $now.format('yyyy-MM-dd') }}
   ```

### Solution 2: Add Transform Node Before Pinecone

Add a Function or Code node before Pinecone to properly format the metadata:

```javascript
// n8n Function Node Code
const items = $input.all();

return items.map(item => {
  // Extract metadata fields
  const metadata = {
    title: item.json.title || '',
    source: item.json.source || '',
    category: item.json.category || 'uncategorized',
    subcategory: item.json.subcategory || '',
    date_scraped: new Date().toISOString().split('T')[0],
    content_type: item.json.content_type || 'text',
    tags: Array.isArray(item.json.tags) ? item.json.tags : []
  };
  
  // Ensure all metadata values are strings or numbers (Pinecone requirement)
  Object.keys(metadata).forEach(key => {
    if (Array.isArray(metadata[key])) {
      metadata[key] = metadata[key].join(',');
    } else if (typeof metadata[key] === 'object') {
      metadata[key] = JSON.stringify(metadata[key]);
    }
  });
  
  return {
    json: {
      id: item.json.id || `doc_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`,
      text: item.json.content || item.json.text,
      metadata: metadata
    }
  };
});
```

### Solution 3: Python Script to Fix Existing Data

If you already have data in Pinecone with blob metadata, use this script to fix it:

```python
# fix_pinecone_metadata.py
import os
import json
from pinecone import Pinecone
from tqdm import tqdm

# Initialize Pinecone
pc = Pinecone(api_key=os.getenv('PINECONE_API_KEY'))
index = pc.Index('personaforge-knowledge')

# Fetch all vectors with blob metadata
results = index.query(
    vector=[0] * 1536,  # Dummy vector for metadata-only query
    top_k=10000,
    include_metadata=True
)

# Fix each vector's metadata
for match in tqdm(results['matches']):
    if 'blob' in match['metadata']:
        try:
            # Parse the blob
            parsed_metadata = json.loads(match['metadata']['blob'])
            
            # Update with parsed metadata
            index.update(
                id=match['id'],
                metadata=parsed_metadata
            )
        except json.JSONDecodeError:
            print(f"Failed to parse metadata for {match['id']}")
```

### Solution 4: n8n HTTP Request Node Alternative

If the Pinecone node continues to have issues, use an HTTP Request node instead:

1. **Node Configuration**:
   - Method: POST
   - URL: `https://{{index-name}}.svc.{{environment}}.pinecone.io/vectors/upsert`
   - Headers:
     - Api-Key: {{PINECONE_API_KEY}}
     - Content-Type: application/json
   
2. **Body**:
   ```json
   {
     "vectors": [
       {
         "id": "{{ $json.id }}",
         "values": {{ $json.embedding }},
         "metadata": {
           "title": "{{ $json.title }}",
           "source": "{{ $json.source }}",
           "category": "{{ $json.category }}"
         }
       }
     ]
   }
   ```

## Verification

After implementing the fix, verify your metadata is stored correctly:

```python
# verify_metadata.py
import os
from pinecone import Pinecone

pc = Pinecone(api_key=os.getenv('PINECONE_API_KEY'))
index = pc.Index('personaforge-knowledge')

# Test query with metadata filter
results = index.query(
    vector=[0] * 1536,
    top_k=5,
    include_metadata=True,
    filter={"category": {"$eq": "therapy"}}
)

for match in results['matches']:
    print(f"ID: {match['id']}")
    print(f"Metadata: {json.dumps(match['metadata'], indent=2)}")
    print("-" * 50)
```

## Prevention

1. **Always validate metadata structure before insertion**
2. **Use fixed collection mode in n8n instead of JSON mode**
3. **Test with a small batch first**
4. **Monitor your Pinecone index regularly**

## Still Having Issues?

1. Check Pinecone logs in their dashboard
2. Verify your API key has write permissions
3. Ensure metadata values are strings, numbers, or lists (not nested objects)
4. Contact support with your workflow export and example data
