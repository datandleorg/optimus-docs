# API Gateway

The central entry point for the Agent Platform, handling authentication, routing, and request management across all microservices.

## 🏗️ Overview

The API Gateway serves as the primary interface for all client interactions with the Agent Platform. It provides centralized authentication, request routing, rate limiting, and API documentation.

## 🚀 Features

- **🔐 Centralized Authentication**: JWT, Google OAuth 2.0, and API key validation
- **🛣️ Request Routing**: Routes requests to appropriate microservices
- **⚡ Rate Limiting**: Configurable rate limiting per route and user
- **💾 Caching**: Redis-based caching for improved performance
- **📚 API Documentation**: Swagger/OpenAPI documentation
- **🔌 Real-time Communication**: WebSocket support for live updates
- **📁 File Upload**: Multer-based file upload processing
- **📊 Monitoring**: Prometheus metrics and comprehensive logging

## 🛠️ Technology Stack

- **Framework**: Node.js with Express.js
- **Authentication**: Passport.js, JWT, Google OAuth
- **Database**: PostgreSQL (primary), MongoDB (secondary)
- **Caching**: Redis with connect-redis
- **Documentation**: Swagger/OpenAPI
- **Monitoring**: Winston logging, Prometheus metrics
- **Security**: Helmet.js, CORS, express-rate-limit

## 📋 Prerequisites

- Node.js (v18+)
- PostgreSQL (v13+)
- MongoDB (v5+)
- Redis (v6+)
- Docker (optional)

## 🚀 Quick Start

### Local Development

1. **Install dependencies**
   ```bash
   npm install
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Run database migrations**
   ```bash
   npm run migrate
   ```

4. **Start the service**
   ```bash
   # Development mode
   npm run dev
   
   # Production mode
   npm start
   ```

### Docker Deployment

```bash
# Build the image
docker build -t api-gateway .

# Run the container
docker run -p 8080:8080 --env-file .env api-gateway
```

## 🔧 Configuration

### Environment Variables

```env
# Server Configuration
PORT=8080
NODE_ENV=development

# Database Configuration
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_DB=optimus
POSTGRES_USER=admin
POSTGRES_PASSWORD=password

MONGODB_HOST=localhost
MONGODB_PORT=27017
MONGODB_DB=optimus

# Redis Configuration
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# JWT Configuration
JWT_SECRET=your-jwt-secret
JWT_EXPIRES_IN=24h

# Google OAuth
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# Service URLs
WORKFLOW_SERVICE_URL=http://localhost:8001
RAG_SERVICE_URL=http://localhost:8002
MODEL_SERVICE_URL=http://localhost:8003
```

## 📚 API Endpoints

### Authentication

- `POST /api/v1/auth/login` - User login
- `POST /api/v1/auth/register` - User registration
- `POST /api/v1/auth/google` - Google OAuth login
- `POST /api/v1/auth/refresh` - Refresh JWT token
- `POST /api/v1/auth/logout` - User logout

### Workflows

- `GET /api/v1/workflows` - List workflows
- `POST /api/v1/workflows` - Create workflow
- `GET /api/v1/workflows/:id` - Get workflow
- `PUT /api/v1/workflows/:id` - Update workflow
- `DELETE /api/v1/workflows/:id` - Delete workflow
- `POST /api/v1/workflows/:id/execute` - Execute workflow

### Agents

- `GET /api/v1/agents` - List agents
- `POST /api/v1/agents` - Create agent
- `GET /api/v1/agents/:id` - Get agent
- `PUT /api/v1/agents/:id` - Update agent
- `DELETE /api/v1/agents/:id` - Delete agent

### Documents

- `GET /api/v1/documents` - List documents
- `POST /api/v1/documents` - Upload documents
- `GET /api/v1/documents/:id` - Get document
- `DELETE /api/v1/documents/:id` - Delete document
- `POST /api/v1/documents/:id/index` - Index document

### Chat

- `GET /api/v1/chat` - List conversations
- `POST /api/v1/chat` - Create conversation
- `GET /api/v1/chat/:id/messages` - Get messages
- `POST /api/v1/chat/:id/messages` - Send message

### Analytics

- `GET /api/v1/analytics/workflows` - Workflow analytics
- `GET /api/v1/analytics/documents` - Document analytics
- `GET /api/v1/analytics/usage` - Usage analytics

## 🔐 Authentication

### JWT Authentication

```bash
# Login
curl -X POST http://localhost:8080/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "password"
  }'

# Use token in subsequent requests
curl -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  http://localhost:8080/api/v1/workflows
```

### API Key Authentication

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  -H "user-id: YOUR_USER_ID" \
  -H "x-account-id: YOUR_ACCOUNT_ID" \
  http://localhost:8080/api/v1/workflows
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
npm run logs

# View specific service logs
docker logs api-gateway
```

## 🧪 Testing

### Run Tests

```bash
# Unit tests
npm test

# Integration tests
npm run test:integration

# E2E tests
npm run test:e2e

# Coverage report
npm run test:coverage
```

### Test Specific Features

```bash
# Test authentication
npm run test:e2e:auth

# Test workflows
npm run test:e2e:workflow

# Test chat
npm run test:e2e:chat
```

## 🚀 Deployment

### Docker

```bash
# Build image
docker build -t api-gateway .

# Run with environment file
docker run -p 8080:8080 --env-file .env api-gateway
```

### Kubernetes

```bash
# Deploy to Kubernetes
helm install api-gateway ./k8s/helm/services/api-gateway -n optimus-dev

# Update deployment
helm upgrade api-gateway ./k8s/helm/services/api-gateway -n optimus-dev
```

## 🔧 Development

### Project Structure

```
src/
├── app.js                 # Main application file
├── server.js             # Server entry point
├── config/               # Configuration files
│   ├── database.js       # Database configuration
│   ├── logger.js         # Logging configuration
│   └── metrics.js        # Metrics configuration
├── controllers/          # Request handlers
├── middleware/           # Custom middleware
├── models/              # Data models
├── routes/              # API routes
├── services/            # Business logic
├── utils/               # Utility functions
└── validations/         # Request validation
```

### Adding New Routes

1. Create route file in `src/routes/`
2. Add controller in `src/controllers/`
3. Register route in `src/app.js`
4. Add validation in `src/validations/`
5. Write tests in `tests/`

### Code Style

```bash
# Lint code
npm run lint

# Fix linting issues
npm run lint:fix

# Format code
npm run format
```

## 📚 Documentation

- [API Documentation](./docs/api-routes.md)
- [Authentication Guide](./docs/authentication.md)
- [Database Schema](./docs/database-schema.md)
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

