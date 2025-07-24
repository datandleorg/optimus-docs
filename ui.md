# UI (User Interface)

The web-based user interface for the Agent Platform, providing a modern, responsive interface for managing workflows, agents, documents, and analytics.

## 🏗️ Overview

The UI is a React-based web application that provides a comprehensive interface for interacting with the Agent Platform. It features a modern design with real-time updates, workflow building capabilities, and comprehensive analytics dashboards.

## 🚀 Features

- **📱 Responsive Design**: Mobile-first design that works on all devices
- **⚡ Real-time Updates**: Live updates via WebSocket connections
- **🔄 Workflow Builder**: Visual workflow creation and editing
- **📄 Document Management**: Upload and manage documents
- **🤖 Agent Management**: Create and configure AI agents
- **📊 Analytics Dashboard**: Visual analytics and performance metrics
- **🔐 Authentication**: JWT-based authentication with role-based access
- **🎨 Modern UI**: Clean, intuitive interface with Tailwind CSS
- **📈 Real-time Charts**: Interactive charts and visualizations

## 🛠️ Technology Stack

- **Framework**: React.js with Redux
- **Routing**: React Router for navigation
- **Styling**: SCSS with Tailwind CSS
- **State Management**: Redux with Redux Toolkit
- **Real-time**: WebSocket integration
- **Charts**: Chart.js for data visualization
- **Testing**: Jest and React Testing Library
- **Build Tool**: Webpack with Babel

## 📋 Prerequisites

- Node.js (v18+)
- npm or yarn
- Modern web browser
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

3. **Start the development server**
   ```bash
   # Development mode
   npm start
   
   # Production build
   npm run build
   ```

### Docker Deployment

```bash
# Build the image
docker build -t ui .

# Run the container
docker run -p 3000:3000 --env-file .env ui
```

## 🔧 Configuration

### Environment Variables

```env
# API Configuration
REACT_APP_API_URL=http://localhost:8080
REACT_APP_WS_URL=ws://localhost:8080

# Authentication
REACT_APP_AUTH_DOMAIN=your-auth-domain
REACT_APP_AUTH_CLIENT_ID=your-client-id

# Feature Flags
REACT_APP_ENABLE_ANALYTICS=true
REACT_APP_ENABLE_CHAT=true
REACT_APP_ENABLE_WORKFLOWS=true

# External Services
REACT_APP_GOOGLE_ANALYTICS_ID=your-ga-id
REACT_APP_SENTRY_DSN=your-sentry-dsn
```

## 📚 Application Structure

### Core Pages

- **Dashboard**: Overview of workflows, agents, and analytics
- **Workflows**: Workflow management and builder
- **Agents**: AI agent configuration and management
- **Documents**: Document upload and management
- **Analytics**: Usage analytics and performance metrics
- **Settings**: User preferences and account management

### Component Architecture

```
src/
├── components/           # Reusable components
│   ├── atoms/          # Basic UI components
│   ├── molecules/      # Compound components
│   └── organisms/      # Complex components
├── pages/              # Page components
│   ├── dashboard/
│   ├── workflows/
│   ├── agents/
│   └── documents/
├── services/           # API services
├── store/             # Redux store
├── hooks/             # Custom React hooks
├── utils/             # Utility functions
└── styles/            # Global styles
```

## 🎨 UI Components

### Atomic Design System

#### Atoms
- **Button**: Primary, secondary, and tertiary buttons
- **Input**: Text, number, and select inputs
- **Card**: Content containers with various styles
- **Badge**: Status indicators and labels
- **Icon**: SVG icons with consistent styling

#### Molecules
- **SearchBar**: Search input with filters
- **Pagination**: Page navigation controls
- **Modal**: Dialog and overlay components
- **Form**: Form components with validation
- **Table**: Data table with sorting and filtering

#### Organisms
- **Header**: Navigation and user menu
- **Sidebar**: Navigation sidebar
- **WorkflowBuilder**: Visual workflow editor
- **DocumentUploader**: File upload interface
- **AnalyticsChart**: Data visualization components

## 🔄 Workflow Builder

### Visual Workflow Editor

```jsx
// Workflow builder component
import { WorkflowBuilder } from '../components/organisms/WorkflowBuilder';

const WorkflowPage = () => {
  return (
    <div className="workflow-page">
      <WorkflowBuilder
        nodes={workflowNodes}
        edges={workflowEdges}
        onSave={handleSave}
        onExecute={handleExecute}
      />
    </div>
  );
};
```

### Node Types

- **LLM Node**: AI model integration
- **HTTP Node**: API calls and webhooks
- **Code Node**: Custom code execution
- **Conditional Node**: Logic and branching
- **Data Node**: Data processing and transformation

## 📊 Analytics Dashboard

### Real-time Metrics

```jsx
// Analytics dashboard
import { AnalyticsChart } from '../components/organisms/AnalyticsChart';

const Dashboard = () => {
  return (
    <div className="dashboard">
      <AnalyticsChart
        data={workflowMetrics}
        type="line"
        title="Workflow Executions"
      />
      <AnalyticsChart
        data={usageMetrics}
        type="bar"
        title="API Usage"
      />
    </div>
  );
};
```

### Chart Types

- **Line Charts**: Time-series data
- **Bar Charts**: Categorical data
- **Pie Charts**: Proportional data
- **Heatmaps**: Correlation data
- **Gauges**: Progress indicators

## 🔐 Authentication

### JWT Authentication

```jsx
// Authentication hook
import { useAuth } from '../hooks/useAuth';

const ProtectedRoute = ({ children }) => {
  const { isAuthenticated, loading } = useAuth();
  
  if (loading) {
    return <LoadingSpinner />;
  }
  
  return isAuthenticated ? children : <Navigate to="/login" />;
};
```

### Role-based Access

```jsx
// Role-based component
import { usePermissions } from '../hooks/usePermissions';

const AdminPanel = () => {
  const { hasPermission } = usePermissions();
  
  if (!hasPermission('admin')) {
    return <AccessDenied />;
  }
  
  return <AdminDashboard />;
};
```

## ⚡ Real-time Features

### WebSocket Integration

```jsx
// Real-time updates
import { useWebSocket } from '../hooks/useWebSocket';

const WorkflowMonitor = () => {
  const { data, connected } = useWebSocket('/ws/workflow/workflow_123');
  
  return (
    <div className="workflow-monitor">
      <StatusIndicator connected={connected} />
      <WorkflowStatus data={data} />
    </div>
  );
};
```

### Live Updates

- **Workflow Status**: Real-time workflow execution updates
- **Chat Messages**: Live chat functionality
- **Notifications**: Real-time system notifications
- **Analytics**: Live dashboard updates

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

### Test Components

```jsx
// Component test example
import { render, screen } from '@testing-library/react';
import { WorkflowBuilder } from './WorkflowBuilder';

test('renders workflow builder', () => {
  render(<WorkflowBuilder />);
  expect(screen.getByText('Workflow Builder')).toBeInTheDocument();
});
```

## 📊 Performance

### Optimization Techniques

- **Code Splitting**: Lazy loading of components
- **Memoization**: React.memo for expensive components
- **Virtual Scrolling**: For large data sets
- **Image Optimization**: WebP and lazy loading
- **Bundle Optimization**: Tree shaking and minification

### Performance Monitoring

```jsx
// Performance monitoring
import { usePerformance } from '../hooks/usePerformance';

const App = () => {
  usePerformance();
  
  return (
    <div className="app">
      <Router>
        <Routes />
      </Router>
    </div>
  );
};
```

## 🚀 Deployment

### Production Build

```bash
# Create production build
npm run build

# Serve production build
npm run serve
```

### Docker

```bash
# Build production image
docker build -t ui:production .

# Run with nginx
docker run -p 80:80 ui:production
```

### Kubernetes

```bash
# Deploy to Kubernetes
helm install ui ./k8s -n optimus-dev

# Update deployment
helm upgrade ui ./k8s -n optimus-dev
```

## 🔧 Development

### Project Structure

```
src/
├── components/           # UI components
│   ├── atoms/          # Basic components
│   ├── molecules/      # Compound components
│   └── organisms/      # Complex components
├── pages/              # Page components
├── services/           # API services
├── store/              # Redux store
├── hooks/              # Custom hooks
├── utils/              # Utilities
├── styles/             # Global styles
└── tests/              # Test files
```

### Adding New Features

1. Create component in appropriate directory
2. Add to component library
3. Write tests for component
4. Update documentation
5. Add to storybook (if applicable)

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

- [Component Library](./docs/component-library.md)
- [API Integration](./docs/api-integration.md)
- [State Management](./docs/state-management.md)
- [Testing Guide](./docs/testing-guide.md)

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