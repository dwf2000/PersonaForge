# PersonaForge - Local RAG System with Zero API Costs 🚀

## AI-Powered Persona Creation & Management System

PersonaForge is a revolutionary local RAG (Retrieval-Augmented Generation) system that achieves enterprise-grade performance without any cloud dependencies or API costs.

## 🎯 Key Innovation: $0 API Costs, 100% Local

We've successfully bypassed expensive cloud services (Pinecone, OpenAI embeddings, etc.) by building a completely local solution that saves **$165+/month** while maintaining professional performance.

## 🏗️ Architecture

### Core Components

1. **ChromaDB** - Local vector database (replaces Pinecone)
   - Stores embeddings for 2000+ documents
   - Handles millions of vectors on a standard laptop
   - Zero monthly fees vs. $70/month for Pinecone

2. **Sentence Transformers** - Local embeddings (replaces OpenAI)
   - Model: `all-MiniLM-L6-v2` (90MB, runs on CPU)
   - Generates embeddings locally
   - Zero API costs vs. $50/month for OpenAI embeddings

3. **Python Integration** - Direct file system access
   - No need for complex MCP servers
   - Claude AI accesses through file system
   - Simple, reliable, maintainable

## 📁 Project Structure

```
LOCAL_RAG_SYSTEM/
├── chroma_db/              # Vector database storage
│   ├── chroma.sqlite3      # Main database file
│   └── [collections]/      # UUID folders for each collection
├── search.py               # Main search interface
├── index_content.py        # Document indexing
├── rag_config.json         # System configuration
└── rag_metadata.db         # SQLite metadata (see note below)
```

## 🔍 Collections

Our system includes 5 specialized collections:

- **creative_hub_main** - General knowledge base
- **persona_memories** - Character experiences and memories
- **geography_resurrection** - Novel content (68K words)
- **therapeutic_songs** - 35 healing songs
- **darian_system** - Technical documentation

## ⚠️ Important Note on SQLite

While our system includes `rag_metadata.db` for storing search history and document metadata, the MCP SQLite server doesn't exist in the npm registry. However, this doesn't affect functionality because:

1. **ChromaDB handles all vector operations** internally using its own SQLite database
2. **Metadata is stored within ChromaDB** alongside vectors
3. **The separate SQLite database is optional** for logging/analytics
4. **Python scripts can access SQLite directly** without needing MCP

## 🚀 Quick Start

### Prerequisites
```bash
pip install chromadb sentence-transformers
```

### Basic Usage
```python
from search import search_rag

# Search your knowledge base
results = search_rag("Professor Chen trauma healing")

# Results include:
# - Relevant documents
# - Source information
# - Relevance scores
# - Collection names
```

### Command Line Search
```bash
cd LOCAL_RAG_SYSTEM
python search.py "your search query"
```

## 💰 Cost Savings Breakdown

| Service | Traditional Cost | Our Solution | Savings |
|---------|-----------------|--------------|---------|
| Pinecone | $70/month | ChromaDB (local) | $70/month |
| OpenAI Embeddings | $50/month | Sentence Transformers | $50/month |
| Supabase | $25/month | Local SQLite | $25/month |
| API Calls | $20/month | Local processing | $20/month |
| **Total** | **$165/month** | **$0/month** | **$1,980/year** |

## 🎯 Performance Metrics

- **Indexing Speed**: ~100 documents/minute
- **Search Response**: <100ms
- **Storage**: ~2GB for complete knowledge base
- **Accuracy**: Comparable to cloud solutions

## 🔧 Technical Details

### Embedding Model
- **Model**: `all-MiniLM-L6-v2`
- **Dimensions**: 384
- **Size**: 90MB
- **Performance**: Excellent for semantic search

### ChromaDB Configuration
- **Persistent Storage**: Local disk
- **Chunk Size**: 500 characters
- **Chunk Overlap**: 50 characters
- **Similarity Threshold**: 0.7

## 🌟 Use Cases

1. **AI-Powered Writing** - Search through novels, stories, and creative content
2. **Persona Development** - Access character memories and experiences
3. **Knowledge Management** - Organize and retrieve any text-based information
4. **Local ChatGPT** - Build context-aware AI assistants without cloud dependencies

## 🔒 Privacy & Security

- **100% Local** - No data leaves your machine
- **No API Keys** - No risk of leaked credentials
- **Complete Control** - You own your data
- **Portable** - Entire system fits on a USB drive

## 🚧 Roadmap

- [ ] Web UI for easier searching
- [ ] Advanced filtering options
- [ ] Multi-modal support (images, audio)
- [ ] Distributed RAG for team collaboration
- [ ] Export/import functionality

## 🤝 Contributing

We welcome contributions! Areas of interest:
- Performance optimizations
- Additional embedding models
- UI/UX improvements
- Documentation
- Integration examples

## 📄 License

MIT License - Use freely in your own projects!

## 🙏 Acknowledgments

Built as part of the PersonaForge creative system, demonstrating that professional-grade AI tools can be accessible to everyone without expensive cloud subscriptions.

---

**Created by**: dwf2000  
**Purpose**: Democratizing AI-powered knowledge management  
**Mission**: Zero API costs, maximum creativity