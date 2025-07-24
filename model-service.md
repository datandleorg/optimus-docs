# Model Service

The AI model integration service for the Agent Platform, providing LLM access, embedding generation, and usage tracking capabilities.

## 🏗️ Overview

The Model Service handles all AI model interactions including text generation, embedding creation, and comprehensive usage tracking. It provides a unified interface for multiple AI providers and models.

## 🚀 Features

- **🤖 LLM Integration**: Support for multiple AI providers (OpenAI, Anthropic, etc.)
- **🔢 Embedding Generation**: Text-to-vector embedding creation
- **📊 Usage Tracking**: Comprehensive usage and cost tracking
- **💰 Cost Optimization**: Track and optimize AI model usage costs
- **🔧 Model Management**: Manage different AI models and their capabilities
- **⚡ Rate Limiting**: Per-user and per-endpoint rate limiting
- **📈 Analytics**: Usage analytics and performance metrics
- **🛡️ Security**: Secure API key management and validation

## 🛠️ Technology Stack

- **Framework**: FastAPI (Python)
- **Database**: PostgreSQL with SQLAlchemy
- **Vector Database**: Qdrant integration
- **Rate Limiting**: SlowAPI for request rate limiting
- **Monitoring**: Custom metrics and usage tracking
- **Testing**: Pytest for testing

## 📋 Prerequisites

- Python (v3.9+)
- PostgreSQL (v13+)
- Qdrant (v1.0+)
- OpenAI API key
- Anthropic API key (optional)
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

3. **Run database migrations**
   ```bash
   alembic upgrade head
   ```

4. **Start the service**
   ```bash
   # Development mode
   uvicorn app.main:app --reload --host 0.0.0.0 --port 8003
   
   # Production mode
   uvicorn app.main:app --host 0.0.0.0 --port 8003
   ```

### Docker Deployment

```bash
# Build the image
docker build -t model-service .

# Run the container
docker run -p 8003:8003 --env-file .env model-service
```

## 🔧 Configuration

### Environment Variables

```env
# Server Configuration
PORT=8003
HOST=0.0.0.0
DEBUG=true

# Database Configuration
DATABASE_URL=postgresql://user:password@localhost:5432/optimus

# Qdrant Configuration
QDRANT_HOST=localhost
QDRANT_PORT=6333

# OpenAI Configuration
OPENAI_API_KEY=your-openai-api-key
OPENAI_ORGANIZATION=your-organization-id

# Anthropic Configuration
ANTHROPIC_API_KEY=your-anthropic-api-key

# Rate Limiting
RATE_LIMIT_REQUESTS=100
RATE_LIMIT_WINDOW=60

# Authentication
JWT_SECRET=your-jwt-secret
JWT_ALGORITHM=HS256

# Logging
LOG_LEVEL=INFO
LOG_FORMAT=json
```

## 📚 API Endpoints

### LLM Generation

- `POST /v1/llm/generate` - Generate text using LLM models
- `GET /v1/llm/usage` - Get usage statistics
- `GET /v1/llm/models` - List available models

### Embeddings

- `POST /v1/llm/generate-embeddings` - Generate embeddings
- `GET /v1/llm/embedding-usage` - Get embedding usage

### Vector Operations

- `POST /v1/vector/collections` - Create vector collection
- `GET /v1/vector/collections` - List collections
- `GET /v1/vector/collections/{name}` - Get collection info
- `DELETE /v1/vector/collections/{name}` - Delete collection
- `POST /v1/vector/points` - Upsert vectors
- `POST /v1/vector/search` - Search vectors
- `DELETE /v1/vector/points` - Delete vectors

### Models

- `GET /api/v1/models` - List all available models

## 🤖 LLM Integration

### Supported Models

#### OpenAI Models
- **GPT-4**: Advanced language model
- **GPT-3.5-turbo**: Fast and efficient model
- **GPT-4-turbo**: Latest GPT-4 variant

#### Anthropic Models
- **Claude-3-opus**: Most capable model
- **Claude-3-sonnet**: Balanced performance
- **Claude-3-haiku**: Fast and efficient

### Text Generation

```python
# Generate text with OpenAI
response = await llm_service.generate_text(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Explain machine learning."}
    ],
    temperature=0.7,
    max_tokens=1000
)

# Generate text with Anthropic
response = await llm_service.generate_text(
    model="claude-3-sonnet",
    messages=[
        {"role": "user", "content": "Explain machine learning."}
    ],
    temperature=0.7,
    max_tokens=1000
)
```

### Embedding Generation

```python
# Generate embeddings
embeddings = await embedding_service.generate_embeddings(
    model="text-embedding-ada-002",
    texts=[
        "This is the first text.",
        "This is the second text."
    ]
)

# Example response
{
    "embeddings": [
        [0.1, 0.2, 0.3, ...],
        [0.4, 0.5, 0.6, ...]
    ],
    "usage": {
        "prompt_tokens": 10,
        "total_tokens": 10
    },
    "cost": 0.0001
}
```

## 💾 Vector Database

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
            "id": "point_1",
            "vector": [0.1, 0.2, ...],
            "payload": {
                "text": "sample text",
                "metadata": "additional info"
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

## 📊 Usage Tracking

### Usage Statistics

```python
# Get usage statistics
usage_stats = await usage_service.get_usage_stats(
    user_id="user_123",
    start_date="2024-01-01",
    end_date="2024-01-31"
)

# Example response
{
    "total_requests": 1250,
    "total_tokens": 50000,
    "total_cost": 25.50,
    "requests_by_model": {
        "gpt-4": 800,
        "gpt-3.5-turbo": 450
    },
    "cost_by_model": {
        "gpt-4": 20.00,
        "gpt-3.5-turbo": 5.50
    },
    "daily_usage": [
        {"date": "2024-01-15", "requests": 45, "cost": 0.90},
        {"date": "2024-01-16", "requests": 52, "cost": 1.05}
    ]
}
```

### Cost Tracking

```python
# Track request cost
cost = await usage_service.calculate_cost(
    model="gpt-4",
    input_tokens=100,
    output_tokens=50
)

# Example cost calculation
{
    "input_cost": 0.03,    # $0.03 per 1K input tokens
    "output_cost": 0.06,   # $0.06 per 1K output tokens
    "total_cost": 0.09
}
```

## 🔧 Model Management

### Available Models

```python
# Get all available models
models = await model_service.get_all_models()

# Example response
[
    {
        "id": "gpt-4",
        "name": "GPT-4",
        "provider": "openai",
        "type": "chat",
        "max_tokens": 8192,
        "cost_per_1k_input": 0.03,
        "cost_per_1k_output": 0.06,
        "capabilities": ["text-generation", "code-generation"]
    },
    {
        "id": "claude-3-sonnet",
        "name": "Claude 3 Sonnet",
        "provider": "anthropic",
        "type": "chat",
        "max_tokens": 200000,
        "cost_per_1k_input": 0.003,
        "cost_per_1k_output": 0.015,
        "capabilities": ["text-generation", "analysis"]
    }
]
```

## ⚡ Rate Limiting

### Rate Limit Configuration

```python
# Rate limits per endpoint
rate_limits = {
    "/v1/llm/generate": "10/minute",
    "/v1/llm/generate-embeddings": "20/minute",
    "/v1/vector/search": "100/minute"
}

# Per-user rate limiting
user_limits = {
    "premium": "1000/hour",
    "standard": "100/hour",
    "basic": "10/hour"
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

### Test Model Integration

```python
# Test LLM generation
async def test_llm_generation():
    response = await client.post(
        "/v1/llm/generate",
        json={
            "messages": [
                {"role": "user", "content": "Hello"}
            ],
            "model": "gpt-3.5-turbo"
        }
    )
    
    assert response.status_code == 200
    assert "response" in response.json()
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
docker logs model-service

# View specific logs
tail -f logs/model-service.log
```

## 🚀 Deployment

### Docker

```bash
# Build image
docker build -t model-service .

# Run with environment file
docker run -p 8003:8003 --env-file .env model-service
```

### Kubernetes

```bash
# Deploy to Kubernetes
helm install model-service ./k8s -n optimus-dev

# Update deployment
helm upgrade model-service ./k8s -n optimus-dev
```

## 🔧 Development

### Project Structure

```
app/
├── main.py                 # FastAPI application
├── config/                 # Configuration files
│   ├── database.py        # Database configuration
│   ├── logger.py          # Logging configuration
│   └── metrics.py         # Metrics configuration
├── models/                # Data models
│   ├── llm_model.py       # LLM model definitions
│   └── embedding_generator.py
├── routes/                # API routes
│   └── model_routes.py
├── services/              # Business logic
│   ├── llm_provider.py    # LLM provider integration
│   ├── model_service.py   # Model management
│   ├── qdrant_service.py  # Vector database
│   └── usage_tracking.py  # Usage tracking
└── utils/                 # Utility functions
```

### Adding New Providers

1. Create provider class in `app/services/providers/`
2. Implement required methods:
   - `generate_text()` - Text generation
   - `generate_embeddings()` - Embedding generation
   - `get_models()` - List available models
3. Register provider in `app/services/provider_registry.py`
4. Add tests in `tests/services/providers/`

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

- [LLM Integration Guide](./docs/llm-integration.md)
- [Embedding API Reference](./docs/embedding-api.md)
- [Vector Database Guide](./docs/vector-database.md)
- [Usage Tracking Guide](./docs/usage-tracking.md)

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