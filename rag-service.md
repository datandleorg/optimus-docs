# RAG Service

The Retrieval-Augmented Generation (RAG) service for the Agent Platform, providing advanced document processing, vector search, and semantic retrieval capabilities.

## 🏗️ Overview

The RAG Service handles document processing, vector embeddings, and semantic search functionality. It provides the foundation for AI-powered document retrieval and knowledge base management.

## 🚀 Features

- **📄 Document Processing**: Upload and process multiple file formats (PDF, DOCX, TXT)
- **🔍 Vector Search**: Semantic search using vector embeddings
- **🔄 Hybrid Search**: Combine vector and traditional text search
- **📊 Document Chunking**: Intelligent document segmentation
- **💾 Vector Storage**: Qdrant-based vector database integration
- **📁 File Storage**: DigitalOcean Spaces integration
- **📈 Analytics**: Document usage analytics and metrics
- **🔍 Metadata Management**: Comprehensive document metadata tracking

## 🛠️ Technology Stack

- **Framework**: FastAPI (Python)
- **Vector Database**: Qdrant for vector storage and similarity search
- **Document Storage**: DigitalOcean Spaces (S3-compatible)
- **Database**: MongoDB for document metadata
- **File Processing**: PyPDF2, python-docx, textract
- **Embeddings**: OpenAI, Sentence Transformers
- **Monitoring**: Custom metrics collection

## 📋 Prerequisites

- Python (v3.9+)
- MongoDB (v5+)
- Qdrant (v1.0+)
- DigitalOcean Spaces account
- Docker (optional)

## 🚀 Quick Start

### Local Development

1. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Start the service**
   ```bash
   # Development mode
   uvicorn app.main:app --reload --host 0.0.0.0 --port 8002
   
   # Production mode
   uvicorn app.main:app --host 0.0.0.0 --port 8002
   ```

### Docker Deployment

```bash
# Build the image
docker build -t rag-service .

# Run the container
docker run -p 8002:8002 --env-file .env rag-service
```

## 🔧 Configuration

### Environment Variables

```env
# Server Configuration
PORT=8002
HOST=0.0.0.0
DEBUG=true

# Database Configuration
MONGODB_URL=mongodb://localhost:27017/optimus
MONGODB_DB=optimus

# Qdrant Configuration
QDRANT_HOST=localhost
QDRANT_PORT=6333
DOCUMENT_COLLECTION=documents

# DigitalOcean Spaces Configuration
SPACES_KEY=your-spaces-key
SPACES_SECRET=your-spaces-secret
SPACES_ENDPOINT=your-spaces-endpoint
SPACES_BUCKET=your-bucket-name
SPACES_REGION=your-region

# Embedding Configuration
EMBEDDING_MODEL=text-embedding-ada-002
EMBEDDING_DIMENSION=1536

# Authentication
JWT_SECRET=your-jwt-secret
JWT_ALGORITHM=HS256

# Logging
LOG_LEVEL=INFO
LOG_FORMAT=json
```

## 📚 API Endpoints

### Documents

- `GET /api/v1/documents` - List documents with pagination
- `POST /api/v1/documents` - Upload multiple documents
- `GET /api/v1/documents/{document_id}` - Get document details
- `DELETE /api/v1/documents/{document_id}` - Delete document
- `POST /api/v1/documents/{document_id}/index` - Index document for search

### Search

- `GET /api/v1/documents/{document_id}/search` - Search within document
- `GET /api/v1/documents/{document_id}/hybrid_search` - Hybrid search
- `POST /api/v1/search` - Global search across all documents

### Analytics

- `GET /api/v1/analytics/documents` - Document analytics
- `GET /api/v1/analytics/search` - Search analytics

## 📄 Document Processing

### Supported File Types

- **PDF**: Text extraction and processing
- **DOCX**: Microsoft Word documents
- **TXT**: Plain text files
- **CSV**: Comma-separated values
- **JSON**: Structured data files

### Processing Pipeline

1. **File Upload**: Files uploaded to DigitalOcean Spaces
2. **Text Extraction**: Extract text from various file formats
3. **Chunking**: Split documents into manageable chunks
4. **Embedding Generation**: Create vector embeddings for chunks
5. **Vector Storage**: Store embeddings in Qdrant
6. **Metadata Storage**: Store document metadata in MongoDB

### Document Chunking

```python
# Example chunking configuration
chunking_config = {
    "chunk_size": 1000,
    "chunk_overlap": 200,
    "separators": ["\n\n", "\n", ". ", " "],
    "min_chunk_size": 100
}
```

## 🔍 Search Capabilities

### Vector Search

```python
# Search for similar content
search_results = await document_service.search_document(
    document_id="doc_123",
    query="What is machine learning?",
    limit=10,
    score_threshold=0.7
)
```

### Hybrid Search

```python
# Combine vector and text search
hybrid_results = await document_service.hybrid_search(
    document_id="doc_123",
    query="machine learning algorithms",
    vector_weight=0.7,
    text_weight=0.3,
    limit=10
)
```

### Global Search

```python
# Search across all documents
global_results = await document_service.global_search(
    query="artificial intelligence",
    filters={
        "user_id": "user_123",
        "document_type": "pdf"
    },
    limit=20
)
```

## 💾 Vector Storage

### Qdrant Integration

```python
# Create collection
await qdrant_service.create_collection(
    collection_name="documents",
    vector_size=1536,
    distance=Distance.COSINE
)

# Add vectors
await qdrant_service.upsert_points(
    collection_name="documents",
    points=[
        {
            "id": "chunk_1",
            "vector": [0.1, 0.2, ...],
            "payload": {
                "text": "chunk text",
                "document_id": "doc_123",
                "chunk_index": 0
            }
        }
    ]
)

# Search vectors
results = await qdrant_service.search_points(
    collection_name="documents",
    query_vector=[0.1, 0.2, ...],
    limit=10,
    score_threshold=0.7
)
```

## 📁 File Storage

### DigitalOcean Spaces

```python
# Upload file
file_url = await spaces_service.upload_file(
    file_path="path/to/document.pdf",
    object_key="documents/doc_123.pdf"
)

# Download file
file_content = await spaces_service.download_file(
    object_key="documents/doc_123.pdf"
)

# Delete file
await spaces_service.delete_file(
    object_key="documents/doc_123.pdf"
)
```

## 📊 Analytics

### Document Analytics

```python
# Get document statistics
analytics = await analytics_service.get_document_analytics(
    user_id="user_123",
    account_id="account_456"
)

# Example response
{
    "total_documents": 150,
    "total_size_mb": 1250.5,
    "documents_by_type": {
        "pdf": 80,
        "docx": 45,
        "txt": 25
    },
    "recent_uploads": [
        {"date": "2024-01-15", "count": 5},
        {"date": "2024-01-14", "count": 3}
    ]
}
```

### Search Analytics

```python
# Get search statistics
search_analytics = await analytics_service.get_search_analytics(
    user_id="user_123",
    time_range="last_30_days"
)

# Example response
{
    "total_searches": 1250,
    "average_results": 8.5,
    "popular_queries": [
        {"query": "machine learning", "count": 45},
        {"query": "artificial intelligence", "count": 32}
    ],
    "search_success_rate": 0.92
}
```

## 🧪 Testing

### Run Tests

```bash
# Unit tests
pytest

# Integration tests
pytest tests/integration/

# E2E tests
pytest tests/e2e/

# Coverage report
pytest --cov=app --cov-report=html
```

### Test Document Processing

```python
# Test document upload
async def test_document_upload():
    with open("test_document.pdf", "rb") as f:
        response = await client.post(
            "/api/v1/documents",
            files={"files": ("test_document.pdf", f, "application/pdf")},
            headers={"x-api-key": "test_key"}
        )
    
    assert response.status_code == 200
    assert len(response.json()) == 1
```

## 📊 Monitoring

### Health Checks

- `GET /health` - Service health check
- `GET /ping` - Simple ping endpoint

### Metrics

- `GET /metrics` - Prometheus metrics

### Logs

```bash
# View logs
docker logs rag-service

# View specific logs
tail -f logs/rag-service.log
```

## 🚀 Deployment

### Docker

```bash
# Build image
docker build -t rag-service .

# Run with environment file
docker run -p 8002:8002 --env-file .env rag-service
```

### Kubernetes

```bash
# Deploy to Kubernetes
helm install rag-service ./k8s -n optimus-dev

# Update deployment
helm upgrade rag-service ./k8s -n optimus-dev
```

## 🔧 Development

### Project Structure

```
app/
├── main.py                 # FastAPI application
├── config/                 # Configuration files
│   ├── mongo.py           # MongoDB configuration
│   ├── spaces.py           # DigitalOcean Spaces configuration
│   └── logger.py           # Logging configuration
├── controllers/            # Request handlers
│   ├── document_controller.py
│   └── index_controller.py
├── models/                 # Data models
│   └── document.py
├── routes/                 # API routes
│   └── document.py
├── services/               # Business logic
│   ├── document_processor.py
│   ├── qdrant_service.py
│   └── spaces_service.py
└── utils/                  # Utility functions
```

### Adding New File Types

1. Create processor in `app/services/processors/`
2. Implement required methods:
   - `extract_text()` - Text extraction logic
   - `get_metadata()` - Metadata extraction
   - `validate_file()` - File validation
3. Register processor in `app/services/processor_registry.py`
4. Add tests in `tests/services/processors/`

### Code Style

```bash
# Format code
black app/

# Sort imports
isort app/

# Lint code
flake8 app/
```

## 📚 Documentation

- [Document Processing Guide](./docs/document-processing.md)
- [Search API Reference](./docs/search-api.md)
- [Vector Database Guide](./docs/vector-database.md)
- [Deployment Guide](./docs/deployment.md)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass
6. Submit a pull request

## 📄 License

This project is licensed under the MIT License.

---

**Back to [Master README](../README.md)** 