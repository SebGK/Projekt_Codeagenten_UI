# Tech Context: ThreadBridge Technology Stack

## Programmiersprachen & Versionen

### Core Languages
- **TypeScript**: v5.3+ (für Type Safety und bessere Entwicklererfahrung)
- **JavaScript**: ES2022+ (für Browser-Kompatibilität)
- **Node.js**: v18+ (für VSCode Extension Runtime)

### Frontend Specific
- **HTML5**: Für DOM-Manipulation und Element-Selektion
- **CSS3**: Für Toolbar-Styling und responsive Design
- **WebGL**: Optional für komplexe Screenshot-Erfassung

## Frameworks & Bibliotheken

### Browser Toolbar Stack
```json
{
  "framework": "Preact v10.19+",
  "rationale": "Kompatibel mit Stagewise, kleiner Bundle Size",
  "alternatives": ["React", "Vue", "Framework-agnostic"],
  "dependencies": {
    "@preact/signals": "^1.2.0",
    "@preact/compat": "^17.1.0",
    "clsx": "^2.0.0",
    "zustand": "^4.4.0"
  }
}
```

### VSCode Extension Stack
```json
{
  "base": "VSCode Extension API v1.85+",
  "framework": "Node.js",
  "dependencies": {
    "vscode": "^1.85.0",
    "@types/vscode": "^1.85.0",
    "ws": "^8.14.0",
    "zod": "^3.22.0",
    "uuid": "^9.0.0"
  }
}
```

### Communication Layer
```json
{
  "base": "@stagewise/srpc v0.1.0",
  "rationale": "Bewährte Schema-validierte RPC-Kommunikation",
  "extensions": {
    "@threadbridge/srpc-contract": "^1.0.0",
    "@threadbridge/agent-adapters": "^1.0.0"
  },
  "websocket": "ws + native WebSocket API"
}
```

## Datenbanken & Storage

### Lokale Datenspeicherung
```typescript
interface StorageStrategy {
  // VSCode Extension Storage
  workspace: {
    provider: "VSCode Workspace Storage API";
    use_case: "Aktive Threads, User Preferences";
    capacity: "Unlimited per Workspace";
  };
  
  // File System Storage
  filesystem: {
    provider: "Node.js fs + JSON";
    location: ".threadbridge/ directory";
    use_case: "Thread History, Context Archive";
    format: "JSON + optional compression";
  };
  
  // Browser Storage
  browser: {
    provider: "IndexedDB + localStorage";
    use_case: "UI State, Recent Contexts";
    capacity: "5-50MB per domain";
  };
}
```

### Optionale Cloud Storage
```typescript
interface CloudStorageOptions {
  // Plugin-basierte Erweiterung
  providers: ["Custom API", "GitHub Gists", "Cloud Storage APIs"];
  implementation: "Plugin System";
  encryption: "Client-side AES-256";
}
```

## Build-Tools & Development Environment

### Monorepo Setup (Stagewise-kompatibel)
```json
{
  "package_manager": "pnpm v8+",
  "monorepo_tool": "Turborepo",
  "workspace_structure": {
    "packages/": {
      "toolbar/": "Browser Toolbar Package",
      "extension/": "VSCode Extension",
      "shared/": "Shared Types & Utilities",
      "agents/": "Agent Adapter Implementations"
    },
    "apps/": {
      "demo/": "Demo Application",
      "docs/": "Documentation Site"
    }
  }
}
```

### Build Configuration
```javascript
// turbo.json
{
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", "out/**"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    },
    "test": {
      "dependsOn": ["build"],
      "outputs": ["coverage/**"]
    },
    "lint": {
      "outputs": []
    }
  }
}
```

### Bundle Tools
- **Extension**: esbuild (für schnelle Builds und kleine Bundle Size)
- **Toolbar**: Vite (für optimale DX und HMR)
- **Types**: TypeScript Compiler API

## Test-Frameworks & Tools

### Testing Strategy
```typescript
interface TestingStack {
  // Unit Testing
  unit: {
    framework: "Vitest";
    coverage: "@vitest/coverage-c8";
    mocking: "@vitest/spy + MSW";
  };
  
  // Integration Testing
  integration: {
    framework: "Vitest + @testing-library";
    browser: "Happy DOM";
    vscode: "@vscode/test-electron";
  };
  
  // E2E Testing
  e2e: {
    framework: "Playwright";
    browsers: ["Chromium", "Firefox", "WebKit"];
    scenarios: "Multi-Agent Workflows";
  };
  
  // Visual Testing
  visual: {
    framework: "Playwright + Percy/Chromatic";
    components: "Storybook Integration";
  };
}
```

### Quality Assurance Tools
```json
{
  "linting": {
    "typescript": "ESLint + @typescript-eslint",
    "code_style": "Prettier",
    "imports": "eslint-plugin-import"
  },
  "static_analysis": {
    "security": "ESLint Security Plugin",
    "performance": "Bundle Analyzer",
    "dependencies": "npm audit + Snyk"
  },
  "code_quality": {
    "sonarqube": "Optional für größere Teams",
    "codecov": "Coverage Tracking",
    "commitlint": "Conventional Commits"
  }
}
```

## Setup-Anleitung für lokale Entwicklung

### Prerequisites
```bash
# Node.js & Package Manager
node --version  # v18+
pnpm --version  # v8+

# VSCode & Extensions
code --version  # v1.85+
# Required Extensions:
# - Cursor (falls vorhanden)
# - Windsurf (falls vorhanden)
```

### Repository Setup
```bash
# Clone & Setup
git clone https://github.com/your-org/threadbridge.git
cd threadbridge
pnpm install

# Build all packages
pnpm build

# Start development
pnpm dev
```

### Development Workflow
```bash
# Start all development servers
pnpm dev              # Startet Toolbar, Extension & Demo

# Einzelne Pakete
pnpm dev --filter=toolbar     # Nur Toolbar
pnpm dev --filter=extension   # Nur Extension
pnpm dev --filter=demo        # Nur Demo App

# Testing
pnpm test                     # Unit & Integration Tests
pnpm test:e2e                 # E2E Tests mit Playwright
pnpm test:coverage            # Coverage Report

# Quality Checks
pnpm lint                     # ESLint + Prettier
pnpm type-check              # TypeScript Compiler
pnpm audit                   # Security Audit
```

### Extension Development Setup
```bash
# VSCode Extension Development
cd packages/extension
pnpm dev                      # Kompiliert Extension
# F5 in VSCode startet Extension Host

# Debugging Setup
# .vscode/launch.json bereits konfiguriert
# Breakpoints in TypeScript möglich
```

## CI/CD Pipeline

### GitHub Actions Workflow
```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v2
      - uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'pnpm'
      
      - run: pnpm install
      - run: pnpm build
      - run: pnpm test
      - run: pnpm lint
      - run: pnpm type-check
      
      # E2E Tests
      - name: Install Playwright
        run: pnpm exec playwright install
      - run: pnpm test:e2e
      
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: pnpm audit
      # Optional: Snyk Security Scan
```

### Release Pipeline
```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    tags: ['v*']

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: pnpm install
      - run: pnpm build
      - run: pnpm test
      
      # VSCode Extension Publishing
      - name: Publish Extension
        run: pnpm vsce publish
        env:
          VSCE_PAT: ${{ secrets.VSCODE_MARKETPLACE_TOKEN }}
      
      # NPM Package Publishing
      - name: Publish Packages
        run: pnpm changeset publish
        env:
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## Deployment-Umgebungen & Konfigurationen

### Development Environment
```typescript
interface DevConfig {
  extension: {
    port: 5746; // Kompatibel mit Stagewise
    debug: true;
    hot_reload: true;
  };
  
  toolbar: {
    dev_server: "http://localhost:3000";
    auto_inject: true;
    mock_agents: true;
  };
}
```

### Production Environment
```typescript
interface ProdConfig {
  extension: {
    port: 5746;
    debug: false;
    telemetry: "opt-in";
    update_check: true;
  };
  
  toolbar: {
    cdn: "unpkg or jsDelivr";
    version_pinning: true;
    fallback_mode: true;
  };
}
```

### Docker Development (Optional)
```dockerfile
# Dockerfile.dev
FROM node:18-alpine
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN npm install -g pnpm
RUN pnpm install
COPY . .
CMD ["pnpm", "dev"]
```

## Technische Einschränkungen

### Browser Compatibility
```json
{
  "supported_browsers": {
    "Chrome": "v100+",
    "Firefox": "v100+",
    "Safari": "v15+",
    "Edge": "v100+"
  },
  "limitations": {
    "ie": "Not Supported",
    "mobile": "Limited Support (no DOM selection)",
    "webview": "Depends on Host Implementation"
  }
}
```

### VSCode Compatibility
```json
{
  "vscode_version": "1.85+",
  "extension_host": "Node.js Runtime",
  "limitations": {
    "web_vscode": "Limited Support (no fs access)",
    "codespaces": "Network restrictions possible",
    "remote_ssh": "Agent access may be limited"
  }
}
```

### Agent API Limitations
```typescript
interface AgentLimitations {
  cursor: {
    mcp_support: "Limited documentation";
    rate_limits: "Unknown";
    context_size: "Model-dependent";
  };
  
  windsurf: {
    api_access: "Reverse-engineering required";
    stability: "Beta software";
    features: "Rapidly changing";
  };
}
```

## Library Management für Context7

### Library Tracking
```typescript
// memory_bank/library.md - Für Context7 MCP
interface LibraryDatabase {
  stagewise: {
    id: "stagewise-io/stagewise";
    description: "Browser toolbar for AI agents";
    last_checked: "2025-01-04";
    key_docs: ["README.md", "docs/quickstart.md"];
  };
  
  vscode_api: {
    id: "microsoft/vscode-docs";
    description: "VSCode Extension API";
    key_docs: ["api/extension-api.md"];
  };
  
  cursor_mcp: {
    id: "anthropic/mcp";
    description: "Model Context Protocol";
    key_docs: ["specification.md", "examples/"];
  };
}
```

### Regular Update Schedule
```bash
# Update-Skript für Dependencies
#!/bin/bash
# scripts/update-deps.sh

echo "Updating dependencies..."
pnpm update --interactive

echo "Checking for security vulnerabilities..."
pnpm audit

echo "Running tests after update..."
pnpm test

echo "Update complete. Review changes and commit."
```

## Performance & Monitoring

### Performance Targets
```typescript
interface PerformanceTargets {
  toolbar: {
    initial_load: "<2s";
    dom_selection: "<100ms";
    context_capture: "<500ms";
    memory_usage: "<50MB";
  };
  
  extension: {
    startup_time: "<1s";
    message_latency: "<50ms";
    thread_sync: "<200ms";
    cpu_usage: "<5% baseline";
  };
  
  agents: {
    connection_time: "<3s";
    response_time: "Agent-dependent";
    throughput: ">10 messages/minute";
  };
}
```

### Monitoring & Telemetry
```typescript
interface MonitoringStack {
  local: {
    logging: "Console + File (dev mode)";
    metrics: "Performance API";
    debugging: "VSCode Debugger + DevTools";
  };
  
  production: {
    telemetry: "VSCode Telemetry API (opt-in)";
    error_tracking: "Local error logs";
    analytics: "Anonymous usage metrics";
  };
}
```

## Security Considerations

### Data Protection
```typescript
interface SecurityMeasures {
  data_transmission: {
    encryption: "TLS 1.3 for external, localhost for internal";
    validation: "Zod schema validation";
    sanitization: "HTML/Script escaping";
  };
  
  data_storage: {
    local: "OS-level permissions";
    sensitive: "No passwords/secrets stored";
    cleanup: "Automatic cleanup of old data";
  };
  
  code_execution: {
    sandbox: "VSCode Extension Host";
    permissions: "Minimal required permissions";
    dependencies: "Regular security audits";
  };
}
```

### Privacy Compliance
```typescript
interface PrivacyCompliance {
  data_collection: {
    principle: "Minimal data collection";
    consent: "Explicit opt-in for telemetry";
    transparency: "Clear data usage documentation";
  };
  
  user_rights: {
    access: "Local data access via file system";
    deletion: "Manual cleanup commands";
    portability: "JSON export functionality";
  };
}
