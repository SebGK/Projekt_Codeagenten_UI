# Library Database für Context7 MCP

## Zweck
Diese Datei verwaltet Library-IDs und wichtige Referenzen für Context7 MCP-Suchen, um aktuelle Dokumentation und Best Practices für ThreadBridge-Entwicklung zu erhalten.

## Core Libraries

### Stagewise (Basis-Projekt)
```json
{
  "id": "stagewise-io/stagewise",
  "description": "Browser toolbar for AI agents - unsere Basis",
  "last_checked": "2025-01-04",
  "priority": "high",
  "key_docs": [
    "README.md",
    "docs/quickstart.md",
    "packages/toolbar/README.md",
    "packages/extension/README.md"
  ],
  "search_terms": ["srpc", "browser toolbar", "agent communication", "dom selection"]
}
```

### VSCode Extension API
```json
{
  "id": "microsoft/vscode-docs",
  "description": "VSCode Extension Development",
  "key_docs": [
    "api/extension-api.md",
    "api/working-with-extensions.md",
    "extension-guides/webview.md",
    "extension-guides/testing.md"
  ],
  "search_terms": ["extension api", "webview", "storage", "commands"]
}
```

### Model Context Protocol (MCP)
```json
{
  "id": "anthropic/mcp",
  "description": "Protocol für AI Agent Communication",
  "key_docs": [
    "specification.md",
    "examples/cursor-integration.md",
    "transport/websocket.md"
  ],
  "search_terms": ["mcp protocol", "cursor integration", "agent communication"]
}
```

## Framework & Tool Libraries

### TypeScript Best Practices
```json
{
  "id": "microsoft/TypeScript",
  "description": "TypeScript Language & Best Practices",
  "key_docs": [
    "handbook/project-config/compiler-options.md",
    "handbook/modules.md",
    "handbook/typescript-in-5-minutes.md"
  ],
  "search_terms": ["typescript config", "strict mode", "module resolution"]
}
```

### Preact (Toolbar Framework)
```json
{
  "id": "preactjs/preact",
  "description": "React-kompatibles Framework für kleine Bundle Size",
  "key_docs": [
    "guide/v10/getting-started.md",
    "guide/v10/typescript.md",
    "guide/v10/hooks.md"
  ],
  "search_terms": ["preact hooks", "signals", "typescript setup"]
}
```

### Zod (Schema Validation)
```json
{
  "id": "colinhacks/zod",
  "description": "TypeScript-first schema validation",
  "key_docs": [
    "README.md",
    "ERROR_HANDLING.md",
    "PRIMITIVES.md"
  ],
  "search_terms": ["zod schema", "validation", "typescript"]
}
```

## Testing Libraries

### Vitest
```json
{
  "id": "vitest-dev/vitest",
  "description": "Vite-native Unit Testing Framework",
  "key_docs": [
    "guide/features.md",
    "config/index.md",
    "api/index.md"
  ],
  "search_terms": ["vitest config", "mocking", "coverage"]
}
```

### Playwright
```json
{
  "id": "microsoft/playwright",
  "description": "E2E Testing Framework",
  "key_docs": [
    "docs/intro.md",
    "docs/test-assertions.md",
    "docs/browsers.md"
  ],
  "search_terms": ["playwright e2e", "browser testing", "vscode extension testing"]
}
```

### Testing Library
```json
{
  "id": "testing-library/dom-testing-library",
  "description": "Simple and complete testing utilities",
  "key_docs": [
    "docs/queries/about.md",
    "docs/api-queries.md",
    "docs/guide-which-query.md"
  ],
  "search_terms": ["testing library", "dom testing", "user events"]
}
```

## Build Tools & DevOps

### Turborepo
```json
{
  "id": "vercel/turbo",
  "description": "Monorepo Build System",
  "key_docs": [
    "docs/handbook/what-is-a-monorepo.md",
    "docs/handbook/package-installation.md",
    "docs/reference/configuration.md"
  ],
  "search_terms": ["turborepo config", "monorepo", "caching"]
}
```

### esbuild
```json
{
  "id": "evanw/esbuild",
  "description": "Fast JavaScript bundler für VSCode Extensions",
  "key_docs": [
    "api/index.md",
    "content-types/index.md",
    "api/build.md"
  ],
  "search_terms": ["esbuild", "bundling", "typescript"]
}
```

### Vite
```json
{
  "id": "vitejs/vite",
  "description": "Frontend Build Tool für Toolbar",
  "key_docs": [
    "guide/index.md",
    "config/index.md",
    "guide/build.md"
  ],
  "search_terms": ["vite config", "hmr", "library mode"]
}
```

## Agent-Specific Libraries

### Cursor Integration
```json
{
  "id": "cursor-ai/cursor",
  "description": "Cursor AI Editor - falls öffentliche Docs verfügbar",
  "key_docs": [
    "docs/api.md",
    "docs/extensions.md",
    "docs/mcp-integration.md"
  ],
  "search_terms": ["cursor api", "cursor extension", "mcp"]
}
```

### Windsurf Integration
```json
{
  "id": "windsurf-ai/windsurf",
  "description": "Windsurf AI Agent - falls öffentliche Docs verfügbar",
  "key_docs": [
    "docs/api.md",
    "docs/integration.md"
  ],
  "search_terms": ["windsurf api", "windsurf integration"]
}
```

## Security & Best Practices

### OWASP Guidelines
```json
{
  "id": "OWASP/owasp-top-ten",
  "description": "Web Application Security",
  "key_docs": [
    "2021/docs/A01_2021-Broken_Access_Control.md",
    "2021/docs/A03_2021-Injection.md",
    "2021/docs/A07_2021-Identification_and_Authentication_Failures.md"
  ],
  "search_terms": ["web security", "injection", "authentication"]
}
```

### Node.js Security
```json
{
  "id": "nodejs/security-wg",
  "description": "Node.js Security Best Practices",
  "key_docs": [
    "docs/security-best-practices.md",
    "docs/threat-model.md"
  ],
  "search_terms": ["nodejs security", "dependency security"]
}
```

## Browser APIs

### Web APIs
```json
{
  "id": "mdn/web-docs",
  "description": "MDN Web API Documentation",
  "key_docs": [
    "Web/API/Document_Object_Model",
    "Web/API/WebSocket",
    "Web/API/IndexedDB_API"
  ],
  "search_terms": ["dom api", "websocket", "indexeddb"]
}
```

### WebExtension APIs
```json
{
  "id": "mdn/webextensions-examples",
  "description": "Browser Extension Development",
  "key_docs": [
    "content-scripts",
    "background-scripts",
    "messaging"
  ],
  "search_terms": ["webextension", "content script", "messaging"]
}
```

## Update Schedule

### Daily Updates (High Priority)
- `stagewise-io/stagewise` - Basis für unser Projekt
- `anthropic/mcp` - MCP Protokoll Changes
- `microsoft/vscode-docs` - VSCode API Updates

### Weekly Updates (Medium Priority)
- `preactjs/preact` - Framework Updates
- `vitest-dev/vitest` - Testing Framework
- `vercel/turbo` - Build System

### Monthly Updates (Low Priority)
- Security Libraries (OWASP, Node.js Security)
- General Documentation (MDN, TypeScript)
- Tool-specific Updates (esbuild, Vite)

## Search Strategies für Context7

### Aktueller Stand recherchieren
```bash
# Beispiel Context7 Queries
"Get latest documentation for stagewise-io/stagewise focusing on SRPC framework and extension architecture"

"Find current Cursor AI MCP integration examples and API documentation from anthropic/mcp"

"Search microsoft/vscode-docs for VSCode extension WebSocket communication best practices"
```

### Problem-spezifische Recherche
```bash
# Bei konkreten Implementierungsfragen
"Find preactjs/preact TypeScript setup with signals and hooks for browser toolbar component"

"Search vitest-dev/vitest for VSCode extension testing patterns and mocking strategies"

"Get turborepo configuration examples for TypeScript monorepo with browser and Node.js packages"
```

### Security & Performance Research
```bash
# Für Sicherheits- und Performance-Optimierungen
"Find OWASP guidelines for secure WebSocket communication in browser extensions"

"Search evanw/esbuild for optimization strategies for VSCode extension bundling"

"Get browser compatibility data from mdn/web-docs for DOM manipulation APIs"
```

## Notes für Context7 Usage

### Best Practices
1. **Specific Queries**: Immer spezifische Library-IDs und Fokus-Bereiche angeben
2. **Version Awareness**: Nach aktuellen Versionen und Breaking Changes fragen
3. **Context Relevance**: ThreadBridge-spezifische Anwendungsfälle erwähnen
4. **Output Limit**: 2k-10k Token Range für optimale Nutzung

### Query Templates
```bash
# Template für neue Library Research
"Search {library-id} for {specific-topic} with focus on {our-use-case} and provide current best practices and examples"

# Template für Update Check
"Check {library-id} for recent changes, breaking changes, or new features relevant to {our-component}"

# Template für Problem Solving
"Find solutions in {library-id} for {specific-problem} in context of {our-architecture}"
