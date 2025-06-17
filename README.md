# PersonaForge 🧠

An AI-powered persona creation and management system with RAG (Retrieval-Augmented Generation) capabilities for health and therapy applications.

## 🎯 Overview

PersonaForge is a comprehensive system that:
- Scrapes relevant health and therapy data from GitHub and other sources
- Stores structured data in Supabase
- Implements RAG for intelligent persona responses
- Uses Pinecone for vector storage and similarity search

## 🏗️ Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   n8n Workflow  │────▶│    Supabase     │────▶│    Pinecone     │
│   (Scraping)    │     │   (Storage)     │     │  (Vectors)      │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                │                         │
                                ▼                         ▼
                        ┌─────────────────┐     ┌─────────────────┐
                        │   RAG System    │◀────│   LLM (Claude/  │
                        │                 │     │    GPT-4)       │
                        └─────────────────┘     └─────────────────┘
```

## 🚀 Current Status

- ✅ **n8n Workflow**: Successfully scraping health/therapy data from GitHub and other sources
- ✅ **Supabase Integration**: Data being stored successfully
- 🔧 **Pinecone Integration**: Metadata insertion issues (storing blobs instead of structured data)
- 🚧 **RAG System**: In development

## 🐛 Known Issues

### Pinecone Metadata Problem
- **Issue**: Pinecone is storing blob data instead of structured metadata
- **Expected**: Individual metadata fields (title, source, category, etc.)
- **Actual**: Single blob field containing all data
- **Impact**: Cannot filter or search by metadata fields

## 📁 Project Structure

```
PersonaForge/
├── workflows/
│   └── n8n/
│       └── health_therapy_scraper.json
├── backend/
│   ├── rag/
│   │   ├── embeddings.py
│   │   ├── pinecone_client.py
│   │   └── retrieval.py
│   └── api/
│       └── main.py
├── scripts/
│   ├── fix_pinecone_metadata.py
│   └── migrate_data.py
├── docs/
│   ├── SETUP.md
│   ├── TROUBLESHOOTING.md
│   └── API.md
└── config/
    └── .env.example
```

## 🔧 Environment Variables

```env
# Supabase
SUPABASE_URL=your_supabase_url
SUPABASE_ANON_KEY=your_anon_key

# Pinecone
PINECONE_API_KEY=your_pinecone_key
PINECONE_INDEX_NAME=personaforge-knowledge

# OpenAI/Anthropic
OPENAI_API_KEY=your_openai_key
ANTHROPIC_API_KEY=your_anthropic_key

# n8n
N8N_WEBHOOK_URL=your_webhook_url
```

## 📚 Documentation

- [Setup Guide](docs/SETUP.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)
- [API Documentation](docs/API.md)

## 🤝 Contributing

Contributions are welcome! Please read our contributing guidelines before submitting PRs.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.
