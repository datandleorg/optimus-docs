# Chat Widget

A lightweight, embeddable chat widget for the Agent Platform, providing real-time AI chat capabilities that can be easily integrated into any website.

## 🏗️ Overview

The Chat Widget is a web component built with Lit Element that provides a customizable, embeddable chat interface. It supports real-time messaging, file uploads, audio recording, and seamless integration with the Agent Platform.

## 🚀 Features

- **💬 Real-time Chat**: Live messaging with AI agents
- **📁 File Upload**: Support for various file types
- **🎤 Audio Messages**: Voice message recording and transcription
- **🔄 Conversation Management**: Multiple conversation support
- **🎨 Customizable UI**: Configurable appearance and behavior
- **📱 Mobile Responsive**: Works on all device sizes
- **🔌 Easy Integration**: Simple script tag integration
- **⚡ Lightweight**: Minimal footprint and fast loading
- **🔐 Secure**: Encrypted communication and authentication

## 🛠️ Technology Stack

- **Framework**: Lit Element (Web Components)
- **Styling**: Tailwind CSS
- **Real-time**: WebSocket for live chat
- **File Handling**: File upload and audio recording
- **Accessibility**: ARIA-compliant components
- **Build Tool**: Rollup for bundling
- **Testing**: Jest for testing

## 📋 Prerequisites

- Modern web browser with Web Components support
- Agent Platform API access
- WebSocket support
- HTTPS for production (required for audio recording)

## 🚀 Quick Start

### Basic Integration

1. **Add the widget script to your HTML**
   ```html
   <script src="https://your-domain.com/widget/chat-widget.js"></script>
   ```

2. **Add the widget element**
   ```html
   <chat-widget 
     api-key="YOUR_API_KEY"
     app-id="YOUR_APP_ID">
   </chat-widget>
   ```

### Advanced Configuration

```html
<chat-widget 
  api-key="YOUR_API_KEY"
  app-id="YOUR_APP_ID"
  theme="dark"
  position="bottom-right"
  welcome-message="Hello! How can I help you today?"
  show-avatar="true"
  enable-audio="true"
  enable-file-upload="true">
</chat-widget>
```

## 🔧 Configuration

### Widget Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `api-key` | String | Required | Your API key for authentication |
| `app-id` | String | Required | Your application ID |
| `theme` | String | "light" | Widget theme (light/dark) |
| `position` | String | "bottom-right" | Widget position |
| `welcome-message` | String | "Hello!" | Initial welcome message |
| `show-avatar` | Boolean | true | Show agent avatar |
| `enable-audio` | Boolean | true | Enable audio recording |
| `enable-file-upload` | Boolean | true | Enable file uploads |
| `max-file-size` | Number | 10485760 | Max file size in bytes |
| `allowed-file-types` | String | "*" | Comma-separated file types |

### CSS Customization

```css
/* Customize widget appearance */
chat-widget {
  --widget-primary-color: #3b82f6;
  --widget-secondary-color: #64748b;
  --widget-background-color: #ffffff;
  --widget-text-color: #1f2937;
  --widget-border-radius: 12px;
  --widget-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
}
```

## 📚 API Integration

### WebSocket Connection

```javascript
// Widget automatically handles WebSocket connection
const widget = document.querySelector('chat-widget');

// Listen for connection events
widget.addEventListener('connected', (event) => {
  console.log('Widget connected:', event.detail);
});

widget.addEventListener('disconnected', (event) => {
  console.log('Widget disconnected:', event.detail);
});
```

### Message Events

```javascript
// Listen for message events
widget.addEventListener('message-sent', (event) => {
  console.log('Message sent:', event.detail);
});

widget.addEventListener('message-received', (event) => {
  console.log('Message received:', event.detail);
});

widget.addEventListener('typing-started', (event) => {
  console.log('Agent is typing...');
});

widget.addEventListener('typing-stopped', (event) => {
  console.log('Agent stopped typing');
});
```

### File Upload Events

```javascript
// Listen for file upload events
widget.addEventListener('file-uploaded', (event) => {
  console.log('File uploaded:', event.detail);
});

widget.addEventListener('file-error', (event) => {
  console.log('File upload error:', event.detail);
});
```

## 🎨 Customization

### Theme Configuration

```javascript
// Set custom theme
widget.setAttribute('theme', 'dark');

// Custom CSS variables
widget.style.setProperty('--widget-primary-color', '#8b5cf6');
widget.style.setProperty('--widget-background-color', '#1f2937');
```

### Position Options

- `bottom-right` (default)
- `bottom-left`
- `top-right`
- `top-left`
- `center`

### Responsive Design

The widget automatically adapts to different screen sizes:

- **Desktop**: Full chat interface with sidebar
- **Tablet**: Compact chat interface
- **Mobile**: Full-screen chat interface

## 📱 Mobile Features

### Touch Gestures

- **Swipe**: Navigate between conversations
- **Long Press**: Message options
- **Pinch**: Zoom in/out on images
- **Tap**: Quick actions

### Audio Recording

```javascript
// Enable/disable audio recording
widget.setAttribute('enable-audio', 'true');

// Listen for audio events
widget.addEventListener('audio-started', (event) => {
  console.log('Audio recording started');
});

widget.addEventListener('audio-stopped', (event) => {
  console.log('Audio recording stopped');
});
```

## 🔐 Security

### Authentication

```javascript
// Set API key securely
widget.setAttribute('api-key', 'your-api-key');

// Validate API key
widget.addEventListener('auth-error', (event) => {
  console.error('Authentication failed:', event.detail);
});
```

### Data Encryption

- **WebSocket**: WSS (secure WebSocket) in production
- **File Upload**: Encrypted file transfer
- **Messages**: End-to-end encryption (if configured)

## 📊 Analytics

### Usage Tracking

```javascript
// Track widget interactions
widget.addEventListener('widget-opened', (event) => {
  // Track widget open
  analytics.track('widget_opened');
});

widget.addEventListener('message-sent', (event) => {
  // Track message sent
  analytics.track('message_sent', {
    message_length: event.detail.content.length
  });
});
```

### Performance Monitoring

```javascript
// Monitor widget performance
widget.addEventListener('load-complete', (event) => {
  console.log('Widget loaded in:', event.detail.loadTime, 'ms');
});

widget.addEventListener('error', (event) => {
  console.error('Widget error:', event.detail);
});
```

## 🧪 Testing

### Unit Tests

```bash
# Run tests
npm test

# Run tests with coverage
npm run test:coverage

# Run tests in watch mode
npm run test:watch
```

### Integration Tests

```javascript
// Test widget integration
describe('Chat Widget', () => {
  beforeEach(() => {
    document.body.innerHTML = `
      <chat-widget 
        api-key="test-key"
        app-id="test-app">
      </chat-widget>
    `;
  });

  test('widget loads correctly', () => {
    const widget = document.querySelector('chat-widget');
    expect(widget).toBeInTheDocument();
  });
});
```

## 🚀 Deployment

### Build Process

```bash
# Install dependencies
npm install

# Build for development
npm run build:dev

# Build for production
npm run build:prod

# Build for specific environment
npm run build:staging
```

### CDN Deployment

```bash
# Deploy to CDN
npm run deploy:cdn

# Deploy to specific CDN
npm run deploy:cloudflare
```

### Self-hosted Deployment

```bash
# Build widget
npm run build

# Copy files to web server
cp dist/chat-widget.js /var/www/html/widget/
cp dist/chat-widget.css /var/www/html/widget/
```

## 🔧 Development

### Project Structure

```
src/
├── components/           # Widget components
│   ├── chat-button.js   # Chat button component
│   ├── chat-header.js   # Header component
│   ├── chat-messages.js # Messages component
│   ├── chat-input.js    # Input component
│   └── chat-sidebar.js  # Sidebar component
├── styles/              # CSS styles
│   ├── chat-widget.css  # Main widget styles
│   └── tailwind.css     # Tailwind styles
├── utils/               # Utility functions
├── index.js             # Main widget file
└── logger.js            # Logging utility
```

### Adding New Features

1. Create component in `src/components/`
2. Add styles in `src/styles/`
3. Register component in `src/index.js`
4. Write tests in `tests/`
5. Update documentation

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

- [Integration Guide](./docs/integration.md)
- [API Reference](./docs/api-reference.md)
- [Styling Guide](./docs/styling.md)
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