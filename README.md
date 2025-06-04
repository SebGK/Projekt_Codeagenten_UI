# ThreadBridge

> 🌉 A seamless bridge between AI agents and development workflows

ThreadBridge enables contextual conversations across multiple AI agents within your IDE, providing a unified interface for agent collaboration and context sharing.

## 🎯 Vision

ThreadBridge transforms how developers interact with AI agents by providing persistent, contextual threads that span across different AI tools. Instead of losing context when switching between agents, ThreadBridge maintains conversation continuity and enables collaborative problem-solving.

## ✨ Key Features

- **🔗 Agent Agnostic**: Works with Cursor, Windsurf, and extensible to other AI agents
- **🧵 Persistent Threads**: Maintain conversation context across sessions and agents
- **🎯 Context Capture**: Smart selection and capture of relevant code/documentation
- **🤝 Agent Collaboration**: Enable multiple agents to work together on the same problem
- **📝 Thread Management**: Organize, search, and manage conversation threads
- **🔄 Real-time Sync**: Keep threads synchronized across multiple IDE instances

## 🏗️ Architecture

ThreadBridge consists of three main components:

### 1. Browser Toolbar (`packages/toolbar`)
- **Technology**: Preact v10.19+ with Zustand for state management
- **Purpose**: Provides in-browser context capture and thread management UI
- **Integration**: Compatible with Stagewise browser toolbar framework

### 2. VSCode Extension (`packages/extension`)
- **Technology**: Node.js with VSCode Extension API v1.85+
- **Purpose**: Deep IDE integration, file system access, and agent communication
- **Communication**: WebSocket-based RPC using @stagewise/srpc

### 3. Agent Adapters (`packages/agents`)
- **Technology**: TypeScript with Zod schema validation
- **Purpose**: Standardized interfaces for different AI agents
- **Supported**: Cursor MCP, Windsurf API, extensible adapter system

## 🚀 Quick Start

### Prerequisites

- **Node.js**: v18+ 
- **pnpm**: v8+ (recommended package manager)
- **VSCode**: v1.85+ 

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/threadbridge.git
cd threadbridge

# Install dependencies
pnpm install

# Build all packages
pnpm build

# Start development environment
pnpm dev
```

### Development Setup

```bash
# Start all development servers
pnpm dev              # Toolbar, Extension & Demo

# Individual packages
pnpm dev --filter=toolbar     # Browser toolbar only
pnpm dev --filter=extension   # VSCode extension only
pnpm dev --filter=demo        # Demo application only

# Testing
pnpm test                     # Unit & integration tests
pnpm test:e2e                 # End-to-end tests
pnpm test:coverage            # Coverage report

# Quality checks
pnpm lint                     # ESLint + Prettier
pnpm type-check              # TypeScript validation
```

## 📦 Project Structure

```
threadbridge/
├── packages/
│   ├── toolbar/              # Browser toolbar component
│   │   ├── src/
│   │   │   ├── components/   # Preact components
│   │   │   ├── hooks/        # Custom hooks
│   │   │   └── stores/       # Zustand stores
│   │   └── package.json
│   ├── extension/            # VSCode extension
│   │   ├── src/
│   │   │   ├── commands/     # VSCode commands
│   │   │   ├── providers/    # Language providers
│   │   │   └── services/     # Core services
│   │   └── package.json
│   ├── shared/               # Shared utilities
│   │   ├── types/            # TypeScript definitions
│   │   ├── utils/            # Common utilities
│   │   └── schemas/          # Zod validation schemas
│   └── agents/               # Agent adapters
│       ├── cursor/           # Cursor MCP adapter
│       ├── windsurf/         # Windsurf API adapter
│       └── base/             # Base adapter interface
├── apps/
│   ├── demo/                 # Demo application
│   └── docs/                 # Documentation site
├── docs/                     # Project documentation
├── memory_bank/              # Development context & knowledge
└── .taskmaster/              # Task management system
```

## 🔧 Configuration

### Environment Variables

Create a `.env` file in the project root:

```bash
# Copy the example environment file
cp .env.example .env
```

Required environment variables:

```env
# AI Agent API Keys (choose based on your agents)
ANTHROPIC_API_KEY=your_anthropic_key_here
OPENAI_API_KEY=your_openai_key_here

# Optional: Research capabilities
PERPLEXITY_API_KEY=your_perplexity_key_here

# Development settings
NODE_ENV=development
DEBUG=threadbridge:*
```

### VSCode Extension Setup

1. Open the project in VSCode
2. Press `F5` to launch the extension development host
3. The extension will be automatically loaded in the new VSCode window

## 🧪 Testing Strategy

### Unit Testing
- **Framework**: Vitest with @testing-library
- **Coverage**: Target 80%+ coverage for core functionality
- **Mocking**: MSW for API mocking, Vitest spies for function mocking

### Integration Testing
- **Browser**: Happy DOM for lightweight browser environment
- **VSCode**: @vscode/test-electron for extension testing
- **Agents**: Mock agent responses for consistent testing

### End-to-End Testing
- **Framework**: Playwright
- **Scenarios**: Multi-agent workflows, context sharing, thread persistence
- **Browsers**: Chromium, Firefox, WebKit

### Visual Testing
- **Components**: Storybook integration for component documentation
- **Regression**: Playwright screenshots for visual regression testing

## 📚 Development Workflow

ThreadBridge uses TaskMaster for task-driven development:

```bash
# View current tasks
pnpm task-master list

# Get next task to work on
pnpm task-master next

# Mark task as complete
pnpm task-master set-status --id=<task-id> --status=done

# Expand complex tasks into subtasks
pnpm task-master expand --id=<task-id> --research
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Process

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'feat: add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Code Style

- **TypeScript**: Strict mode enabled
- **Linting**: ESLint with @typescript-eslint
- **Formatting**: Prettier with 2-space indentation
- **Commits**: Conventional Commits format

## 🔒 Security & Privacy

### Data Protection
- **Local Storage**: All thread data stored locally by default
- **Encryption**: TLS 1.3 for external communications
- **Validation**: Zod schema validation for all data structures
- **Permissions**: Minimal required VSCode permissions

### Privacy Compliance
- **Minimal Data**: Only necessary data collected
- **Opt-in Telemetry**: Anonymous usage metrics (optional)
- **User Control**: Full control over data storage and sharing
- **Transparency**: Clear documentation of data usage

## 📈 Performance

### Performance Targets
- **Toolbar Load**: < 2 seconds initial load
- **DOM Selection**: < 100ms response time
- **Context Capture**: < 500ms for typical selections
- **Extension Startup**: < 1 second in VSCode
- **Memory Usage**: < 50MB for toolbar, < 100MB for extension

### Optimization Strategies
- **Bundle Splitting**: Code splitting for optimal loading
- **Lazy Loading**: Components and features loaded on demand
- **Caching**: Intelligent caching of context and thread data
- **Debouncing**: User input debouncing for smooth interactions

## 🛠️ Technology Stack

### Core Technologies
- **TypeScript**: v5.3+ for type safety
- **Node.js**: v18+ for runtime environment
- **Preact**: v10.19+ for UI components
- **Vite**: Build tool for frontend
- **esbuild**: Build tool for extension

### Communication
- **WebSockets**: Real-time communication via `ws`
- **RPC**: Schema-validated RPC with @stagewise/srpc
- **Validation**: Zod for runtime type checking

### Development Tools
- **Monorepo**: Turborepo for efficient builds
- **Package Manager**: pnpm for fast, efficient installs
- **Testing**: Vitest + Playwright for comprehensive testing
- **Quality**: ESLint + Prettier + TypeScript

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Stagewise**: For the browser toolbar framework and RPC system
- **VSCode Team**: For the excellent extension API
- **AI Community**: For inspiring the multi-agent collaboration concept

## 📞 Support

- **Documentation**: [docs/](docs/)
- **Issues**: [GitHub Issues](https://github.com/your-org/threadbridge/issues)
- **Discussions**: [GitHub Discussions](https://github.com/your-org/threadbridge/discussions)
- **Email**: threadbridge@your-org.com

---

**Built with ❤️ for the AI-powered development community**
