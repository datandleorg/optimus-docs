# Workflow Service

The core workflow execution engine for the Agent Platform, handling complex workflow definitions, job management, and real-time event processing.

## 🏗️ Overview

The Workflow Service is responsible for executing AI-powered workflows with support for various node types, real-time updates, and distributed job processing. It provides a robust engine for building and running complex automation workflows.

## 🚀 Features

- **🔄 Workflow Engine**: Execute complex workflow definitions with nodes and edges
- **📊 Graph-based Processing**: Support for directed acyclic graphs (DAGs)
- **⚡ Real-time Updates**: WebSocket and Server-Sent Events for live workflow status
- **🔧 Node Types**: Support for LLM, HTTP, Code, Conditional, and custom nodes
- **📋 Job Management**: Handle long-running workflow executions
- **🛡️ Error Handling**: Comprehensive error handling and retry mechanisms
- **📈 Analytics**: Workflow execution analytics and metrics
- **🔌 Event-driven**: Redis Streams for event processing

## 🛠️ Technology Stack

- **Framework**: FastAPI (Python)
- **Database**: MongoDB with Motor (async driver)
- **Message Queue**: Redis Streams and Pub/Sub
- **Authentication**: JWT token validation
- **Monitoring**: Custom metrics collection
- **Testing**: Pytest for testing

## 📋 Prerequisites

- Python (v3.9+)
- MongoDB (v5+)
- Redis (v6+)
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
   uvicorn app.main:app --reload --host 0.0.0.0 --port 8001
   
   # Production mode
   uvicorn app.main:app --host 0.0.0.0 --port 8001
   ```

### Docker Deployment

```bash
# Build the image
docker build -t workflow-service .

# Run the container
docker run -p 8001:8001 --env-file .env workflow-service
```

## 🔧 Configuration

### Environment Variables

```env
# Server Configuration
PORT=8001
HOST=0.0.0.0
DEBUG=true

# Database Configuration
MONGODB_URL=mongodb://localhost:27017/optimus
MONGODB_DB=optimus

# Redis Configuration
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# Authentication
JWT_SECRET=your-jwt-secret
JWT_ALGORITHM=HS256

# Service URLs
API_GATEWAY_URL=http://localhost:8080
MODEL_SERVICE_URL=http://localhost:8003
RAG_SERVICE_URL=http://localhost:8002

# Logging
LOG_LEVEL=INFO
LOG_FORMAT=json
```

## 📚 API Endpoints

### Workflows

- `GET /api/v1/workflows` - List workflows with pagination
- `POST /api/v1/workflows` - Create new workflow
- `GET /api/v1/workflows/{workflow_id}` - Get workflow details
- `PUT /api/v1/workflows/{workflow_id}` - Update workflow
- `DELETE /api/v1/workflows/{workflow_id}` - Delete workflow
- `POST /api/v1/workflows/{workflow_id}/execute` - Execute workflow

### Jobs

- `GET /api/v1/jobs` - List workflow jobs
- `GET /api/v1/jobs/{job_id}` - Get job details
- `POST /api/v1/jobs/{job_id}/cancel` - Cancel job execution
- `GET /api/v1/jobs/{job_id}/logs` - Get job logs

### Nodes

- `GET /api/v1/nodes` - List available node types
- `GET /api/v1/nodes/{node_type}` - Get node type details
- `POST /api/v1/nodes/{node_type}/test` - Test node configuration

### Analytics

- `GET /api/v1/analytics/workflows` - Workflow execution analytics
- `GET /api/v1/analytics/jobs` - Job performance analytics

## 🔄 Workflow Engine

### Node Types

#### LLM Node
```python
{
    "id": "llm_node",
    "type": "llm",
    "config": {
        "model": "gpt-4",
        "prompt": "{{input.prompt}}",
        "temperature": 0.7,
        "max_tokens": 1000
    }
}
```

#### HTTP Node
```python
{
    "id": "http_node",
    "type": "http",
    "config": {
        "method": "POST",
        "url": "https://api.example.com/endpoint",
        "headers": {
            "Authorization": "Bearer {{secrets.api_key}}"
        },
        "body": {
            "data": "{{input.data}}"
        }
    }
}
```

#### Code Node
```python
{
    "id": "code_node",
    "type": "code",
    "config": {
        "language": "python",
        "code": """
def process(input_data):
    result = input_data * 2
    return {"output": result}
        """,
        "input_mapping": {
            "input_data": "{{input.value}}"
        }
    }
}
```

#### Conditional Node
```python
{
    "id": "conditional_node",
    "type": "conditional",
    "config": {
        "condition": "{{input.value}} > 10",
        "true_edge": "success_path",
        "false_edge": "failure_path"
    }
}
```

### Workflow Definition

```python
{
    "name": "Customer Support Workflow",
    "description": "Automated customer support workflow",
    "nodes": [
        {
            "id": "start",
            "type": "start",
            "config": {}
        },
        {
            "id": "classify",
            "type": "llm",
            "config": {
                "model": "gpt-4",
                "prompt": "Classify this customer inquiry: {{input.message}}"
            }
        },
        {
            "id": "route",
            "type": "conditional",
            "config": {
                "condition": "{{classify.output.category}} == 'technical'",
                "true_edge": "technical_support",
                "false_edge": "general_support"
            }
        }
    ],
    "edges": [
        {"source": "start", "target": "classify"},
        {"source": "classify", "target": "route"}
    ]
}
```

## 🔌 Event System

### Redis Streams

The service uses Redis Streams for event processing:

```python
# Publish workflow event
await stream_emitter.publish_workflow_event(
    workflow_id="workflow_123",
    event_type="workflow_started",
    data={"status": "running"}
)

# Subscribe to workflow events
async def handle_workflow_events():
    async for event in stream_consumer.subscribe_workflow_events("workflow_123"):
        print(f"Received event: {event}")
```

### WebSocket Events

Real-time updates via WebSocket:

```javascript
// Client-side WebSocket connection
const ws = new WebSocket('ws://localhost:8001/ws/workflow/workflow_123');

ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    console.log('Workflow update:', data);
};
```

## 📊 Job Management

### Job States

- `pending` - Job is queued for execution
- `running` - Job is currently executing
- `completed` - Job completed successfully
- `failed` - Job failed with error
- `cancelled` - Job was cancelled

### Job Execution

```python
# Create job
job = await workflow_controller.create_job(
    workflow_id="workflow_123",
    input_data={"message": "Hello world"}
)

# Monitor job
job_status = await workflow_controller.get_job_status(job.id)

# Cancel job
await workflow_controller.cancel_job(job.id)
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

### Test Workflows

```python
# Test workflow execution
async def test_workflow_execution():
    workflow = create_test_workflow()
    job = await workflow_controller.execute_workflow(workflow.id, {"input": "test"})
    
    assert job.status == "completed"
    assert job.result["output"] == "expected_result"
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
docker logs workflow-service

# View specific logs
tail -f logs/workflow-service.log
```

## 🚀 Deployment

### Docker

```bash
# Build image
docker build -t workflow-service .

# Run with environment file
docker run -p 8001:8001 --env-file .env workflow-service
```

### Kubernetes

```bash
# Deploy to Kubernetes
helm install workflow-service ./k8s -n optimus-dev

# Update deployment
helm upgrade workflow-service ./k8s -n optimus-dev
```

## 🔧 Development

### Project Structure

```
app/
├── main.py                 # FastAPI application
├── config/                 # Configuration files
│   ├── db.py              # Database configuration
│   ├── logger.py           # Logging configuration
│   └── redis_config.py     # Redis configuration
├── controllers/            # Request handlers
│   ├── workflow_controller.py
│   └── job_controller.py
├── models/                 # Data models
│   ├── workflow.py
│   └── job.py
├── routes/                 # API routes
│   ├── workflow_routes.py
│   └── job_routes.py
├── services/               # Business logic
│   ├── workflow_service.py
│   └── execution_service.py
└── utils/                  # Utility functions
```

### Adding New Node Types

1. Create node class in `app/services/nodes/`
2. Implement required methods:
   - `execute()` - Node execution logic
   - `validate_config()` - Configuration validation
   - `get_schema()` - Node schema definition
3. Register node in `app/services/node_registry.py`
4. Add tests in `tests/services/nodes/`

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

- [Workflow Engine Guide](./docs/workflow-engine.md)
- [Node Types Reference](./docs/node-types.md)
- [API Reference](./docs/api-reference.md)
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