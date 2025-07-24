# Agent Platform

A comprehensive, microservices-based AI agent platform designed for intelligent workflow automation, document processing, and AI-powered interactions.

## 🚀 Overview

The Agent Platform is a modern, scalable solution that combines workflow automation, document processing, and AI capabilities into a unified platform. Built with microservices architecture, it provides a complete ecosystem for creating, managing, and deploying AI agents and workflows.

### Key Features

- 🤖 **AI Agent Management**: Create and configure intelligent AI agents
- 🔄 **Workflow Automation**: Visual workflow builder with drag-and-drop interface
- 📄 **Document Processing**: Advanced RAG (Retrieval-Augmented Generation) capabilities
- 💬 **Real-time Chat**: Live chat widget for customer interactions
- 📊 **Analytics Dashboard**: Comprehensive analytics and monitoring
- 🔐 **Enterprise Security**: Role-based access control and secure authentication
- 📱 **Responsive UI**: Mobile-first design with modern interface
- 🚀 **Scalable Architecture**: Kubernetes-ready microservices deployment

## 🏗️ Architecture

The platform consists of six core microservices:

### Core Services

| Service | Technology | Purpose |
|---------|------------|---------|
| [**API Gateway**](./api-gateway.md) | Node.js/Express | Centralized authentication, routing, and request handling |
| [**Workflow Service**](./workflow-service.md) | FastAPI/Python | Workflow execution engine and job management |
| [**RAG Service**](./rag-service.md) | FastAPI/Python | Document processing and vector search |
| [**Model Service**](./model-service.md) | FastAPI/Python | LLM integration and embedding generation |
| [**UI**](./ui.md) | React/Redux | Web-based user interface |
| [**Widget**](./widget.md) | Lit Element | Embeddable chat widget |

### Technology Stack

- **Backend**: Node.js, Python (FastAPI)
- **Frontend**: React.js, Lit Element
- **Databases**: PostgreSQL, MongoDB, Redis, Qdrant
- **Message Queue**: Redis Streams
- **Deployment**: Kubernetes, Docker
- **Monitoring**: Prometheus, Grafana

## 📋 Prerequisites

Before setting up the Agent Platform, ensure you have:

- **Docker** (v20.10+) and **Docker Compose** (v2.0+)
- **Kubernetes** cluster (v1.24+) with kubectl configured
- **Helm** (v3.8+) for Kubernetes deployments
- **Node.js** (v18+) for local development
- **Python** (v3.9+) for service development
- **Git** for version control

### System Requirements

- **CPU**: 4+ cores
- **RAM**: 8GB+ (16GB recommended)
- **Storage**: 50GB+ available space
- **Network**: Stable internet connection for external dependencies

## 🛠️ Installation

### Quick Start (Docker Compose)

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-org/agent-platform.git
   cd agent-platform
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Start the platform**
   ```bash
   docker-compose up -d
   ```

4. **Access the platform**
   - UI: http://localhost:3000
   - API Gateway: http://localhost:8080
   - API Documentation: http://localhost:8080/docs

### Kubernetes Deployment

1. **Add Helm repositories**
   ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   helm repo update
   ```

2. **Install dependencies**
   ```bash
   # Install databases
   helm install postgresql bitnami/postgresql -n optimus-dev
   helm install mongodb bitnami/mongodb -n optimus-dev
   helm install redis bitnami/redis -n optimus-dev
   helm install qdrant qdrant/qdrant -n optimus-dev
   ```

3. **Deploy services**
   ```bash
   # Deploy API Gateway
   helm install api-gateway ./k8s/helm/services/api-gateway -n optimus-dev

   # Deploy other services
   helm install workflow-service ./workflow-service/k8s -n optimus-dev
   helm install rag-service ./rag-service/k8s -n optimus-dev
   helm install model-service ./model-service/k8s -n optimus-dev
   ```

4. **Deploy UI**
   ```bash
   helm install ui ./ui/k8s -n optimus-dev
   ```

### Local Development Setup

1. **Install dependencies**
   ```bash
   # API Gateway
   cd api-gateway
   npm install

   # Workflow Service
   cd ../workflow-service
   pip install -r requirements.txt

   # RAG Service
   cd ../rag-service
   pip install -r requirements.txt

   # Model Service
   cd ../model-service
   pip install -r requirements.txt

   # UI
   cd ../ui
   npm install
   ```

2. **Set up databases**
   ```bash
   # Start required databases
   docker-compose -f docker-compose.dev.yml up -d postgresql mongodb redis qdrant
   ```

3. **Run migrations**
   ```bash
   # API Gateway migrations
   cd api-gateway
   npm run migrate

   # Model Service migrations
   cd ../model-service
   alembic upgrade head
   ```

4. **Start services**
   ```bash
   # Start all services in development mode
   npm run dev:all
   ```

## 🔧 Configuration

### Environment Variables

Create a `.env` file in the root directory:

```env
# Database Configuration
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_DB=optimus
POSTGRES_USER=admin
POSTGRES_PASSWORD=password

MONGODB_HOST=localhost
MONGODB_PORT=27017
MONGODB_DB=optimus

REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

QDRANT_HOST=localhost
QDRANT_PORT=6333

# API Keys
OPENAI_API_KEY=your-openai-api-key
ANTHROPIC_API_KEY=your-anthropic-api-key

# JWT Configuration
JWT_SECRET=your-jwt-secret
JWT_EXPIRES_IN=24h

# Google OAuth
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# DigitalOcean Spaces (for file storage)
SPACES_KEY=your-spaces-key
SPACES_SECRET=your-spaces-secret
SPACES_ENDPOINT=your-spaces-endpoint
SPACES_BUCKET=your-bucket-name
```

### Service Configuration

Each service has its own configuration files:

- **API Gateway**: `api-gateway/src/config/`
- **Workflow Service**: `workflow-service/app/config/`
- **RAG Service**: `rag-service/app/config/`
- **Model Service**: `model-service/app/config/`

## 🚀 Usage

### Getting Started

1. **Create an Account**
   - Visit the platform UI at http://localhost:3000
   - Sign up with email or Google OAuth
   - Verify your email address

2. **Create Your First Agent**
   - Navigate to the Agents section
   - Click "Create New Agent"
   - Configure agent settings and capabilities

3. **Upload Documents**
   - Go to the Documents section
   - Upload PDF, DOCX, or TXT files
   - Wait for processing and indexing

4. **Build a Workflow**
   - Access the Workflow Builder
   - Drag and drop nodes to create workflows
   - Connect nodes to define execution flow
   - Test and deploy your workflow

### API Usage

#### Authentication

```bash
# Get API key from UI settings
curl -H "x-api-key: YOUR_API_KEY" \
     -H "user-id: YOUR_USER_ID" \
     -H "x-account-id: YOUR_ACCOUNT_ID" \
     http://localhost:8080/api/v1/workflows
```

#### Create a Workflow

```bash
curl -X POST http://localhost:8080/api/v1/workflows \
  -H "Content-Type: application/json" \
  -H "x-api-key: YOUR_API_KEY" \
  -H "user-id: YOUR_USER_ID" \
  -H "x-account-id: YOUR_ACCOUNT_ID" \
  -d '{
    "name": "Customer Support Workflow",
    "description": "Automated customer support workflow",
    "nodes": [
      {
        "id": "start",
        "type": "start",
        "config": {}
      },
      {
        "id": "llm",
        "type": "llm",
        "config": {
          "model": "gpt-4",
          "prompt": "Help customer with their inquiry"
        }
      }
    ],
    "edges": [
      {
        "source": "start",
        "target": "llm"
      }
    ]
  }'
```

#### Upload Documents

```bash
curl -X POST http://localhost:8080/api/v1/documents \
  -H "x-api-key: YOUR_API_KEY" \
  -H "user-id: YOUR_USER_ID" \
  -H "x-account-id: YOUR_ACCOUNT_ID" \
  -F "files=@document.pdf"
```

### Widget Integration

Add the chat widget to your website:

```html
<script src="https://your-domain.com/widget/chat-widget.js"></script>
<chat-widget 
  api-key="YOUR_API_KEY"
  app-id="YOUR_APP_ID">
</chat-widget>
```

## 📚 Documentation

### Component Documentation

- **[API Gateway](./api-gateway/README.md)** - Centralized authentication, routing, and request handling
- **[Workflow Service](./workflow-service/README.md)** - Workflow execution engine and job management
- **[RAG Service](./rag-service/README.md)** - Document processing and vector search
- **[Model Service](./model-service/README.md)** - LLM integration and embedding generation
- **[UI](./ui/README.md)** - Web-based user interface
- **[Chat Widget](./my-widget/README.md)** - Embeddable chat widget

### API Documentation

- **Swagger UI**: http://localhost:8080/docs
- **API Reference**: [API Documentation](./docs/api-reference.md)
- **Authentication Guide**: [Authentication](./docs/authentication.md)

### Architecture Documentation

- **System Architecture**: [Architecture Overview](./ARCHITECTURE_DOCUMENTATION.md)
- **Database Schema**: [Database Schema](./docs/database-schema.md)
- **Deployment Guide**: [Deployment](./docs/deployment.md)

### Development Documentation

- **Contributing Guide**: [Contributing](./CONTRIBUTING.md)
- **Development Setup**: [Development](./docs/development.md)
- **Testing Guide**: [Testing](./docs/testing.md)

## 🧪 Testing

### Run Tests

```bash
# API Gateway tests
cd api-gateway
npm test

# Workflow Service tests
cd ../workflow-service
pytest

# RAG Service tests
cd ../rag-service
pytest

# Model Service tests
cd ../model-service
pytest

# UI tests
cd ../ui
npm test
```

### End-to-End Tests

```bash
# Run all E2E tests
npm run test:e2e

# Run specific test suites
npm run test:e2e:workflow
npm run test:e2e:chat
npm run test:e2e:document
```

## 🔍 Monitoring

### Health Checks

- **API Gateway**: http://localhost:8080/health
- **Workflow Service**: http://localhost:8001/health
- **RAG Service**: http://localhost:8002/health
- **Model Service**: http://localhost:8003/health

### Metrics

- **Prometheus Metrics**: http://localhost:8080/metrics
- **Grafana Dashboard**: http://localhost:3001 (if deployed)

### Logs

```bash
# View service logs
docker-compose logs -f api-gateway
docker-compose logs -f workflow-service
docker-compose logs -f rag-service
docker-compose logs -f model-service
```

## 🚀 Deployment

### Production Deployment

1. **Prepare production environment**
   ```bash
   # Set production environment variables
   export NODE_ENV=production
   export KUBECONFIG=/path/to/production/kubeconfig
   ```

2. **Deploy to production**
   ```bash
   # Deploy all services
   ./scripts/deploy-all.sh production
   ```

3. **Verify deployment**
   ```bash
   # Check service status
   kubectl get pods -n optimus-prod
   kubectl get services -n optimus-prod
   ```

### Scaling

```bash
# Scale services horizontally
kubectl scale deployment api-gateway --replicas=3 -n optimus-prod
kubectl scale deployment workflow-service --replicas=2 -n optimus-prod
kubectl scale deployment rag-service --replicas=2 -n optimus-prod
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](./CONTRIBUTING.md) for details.

### Development Workflow

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Ensure all tests pass
6. Submit a pull request

### Code Style

- **JavaScript/TypeScript**: ESLint + Prettier
- **Python**: Black + isort + flake8
- **React**: ESLint + Prettier

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

## 🆘 Support

### Getting Help

- **Documentation**: Check the [docs](./docs/) directory
- **Issues**: Report bugs on [GitHub Issues](https://github.com/your-org/agent-platform/issues)
- **Discussions**: Join our [GitHub Discussions](https://github.com/your-org/agent-platform/discussions)
- **Email**: support@agent-platform.com

### Community

- **Discord**: [Join our Discord server](https://discord.gg/agent-platform)
- **Twitter**: [Follow us on Twitter](https://twitter.com/agentplatform)
- **Blog**: [Read our blog](https://blog.agent-platform.com)

## 🗺️ Roadmap

### Upcoming Features

- [ ] **Multi-language Support**: Support for multiple languages
- [ ] **Advanced Analytics**: Enhanced analytics and reporting
- [ ] **Mobile App**: Native mobile applications
- [ ] **Enterprise Features**: SSO, LDAP integration
- [ ] **Plugin System**: Extensible plugin architecture
- [ ] **Advanced AI Models**: Support for more AI models

### Version History

- **v1.0.0** - Initial release with core functionality
- **v1.1.0** - Added advanced workflow features
- **v1.2.0** - Enhanced RAG capabilities
- **v1.3.0** - Improved UI/UX and performance

## 🙏 Acknowledgments

- Built with [FastAPI](https://fastapi.tiangolo.com/)
- Powered by [React](https://reactjs.org/)
- Vector storage by [Qdrant](https://qdrant.tech/)
- AI models by [OpenAI](https://openai.com/) and [Anthropic](https://anthropic.com/)

---

**Made with ❤️ by the Agent Platform Team** 