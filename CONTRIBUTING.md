# Contributing to ThreadBridge

Thank you for your interest in contributing to ThreadBridge! This guide will help you get started with contributing to our project.

## 🤝 Code of Conduct

By participating in this project, you agree to abide by our Code of Conduct. Please treat all contributors with respect and foster an inclusive environment.

## 🚀 Getting Started

### Prerequisites

- **Node.js**: v18 or higher
- **pnpm**: v8 or higher (recommended package manager)
- **Git**: Latest version
- **VSCode**: v1.85+ (for extension development)

### Setup Development Environment

1. **Fork the Repository**
   ```bash
   # Fork the repo on GitHub, then clone your fork
   git clone https://github.com/YOUR_USERNAME/threadbridge.git
   cd threadbridge
   ```

2. **Install Dependencies**
   ```bash
   # Install all dependencies
   pnpm install
   ```

3. **Setup Environment Variables**
   ```bash
   # Copy environment template
   cp .env.example .env
   
   # Add your API keys (optional for most contributions)
   # ANTHROPIC_API_KEY=your_key_here
   # OPENAI_API_KEY=your_key_here
   ```

4. **Build the Project**
   ```bash
   # Build all packages
   pnpm build
   
   # Start development servers
   pnpm dev
   ```

## 📋 Development Workflow

### Using TaskMaster

ThreadBridge uses TaskMaster for task-driven development. This helps maintain organized, incremental progress:

```bash
# View available tasks
pnpm task-master list

# Get the next task to work on
pnpm task-master next

# View specific task details
pnpm task-master show <task-id>

# Mark task as complete
pnpm task-master set-status --id=<task-id> --status=done
```

### Branch Strategy

1. **Create a Feature Branch**
   ```bash
   git checkout -b feature/descriptive-name
   # or
   git checkout -b fix/bug-description
   # or
   git checkout -b docs/improvement-description
   ```

2. **Make Your Changes**
   - Follow our coding standards (see below)
   - Write tests for new functionality
   - Update documentation as needed

3. **Test Your Changes**
   ```bash
   # Run all tests
   pnpm test
   
   # Run type checking
   pnpm type-check
   
   # Run linting
   pnpm lint
   
   # Run end-to-end tests
   pnpm test:e2e
   ```

4. **Commit Your Changes**
   ```bash
   # Follow conventional commits format
   git commit -m "feat: add thread persistence functionality"
   git commit -m "fix: resolve context capture memory leak"
   git commit -m "docs: update installation instructions"
   ```

## 🏗️ Project Structure

Understanding the project structure will help you navigate and contribute effectively:

```
threadbridge/
├── packages/
│   ├── toolbar/              # Browser toolbar (Preact + Zustand)
│   ├── extension/            # VSCode extension (Node.js)
│   ├── shared/               # Shared utilities and types
│   └── agents/               # Agent adapters (Cursor, Windsurf)
├── apps/
│   ├── demo/                 # Demo application
│   └── docs/                 # Documentation site
├── docs/                     # Project documentation
├── tests/                    # End-to-end tests
└── .taskmaster/              # Task management system
```

### Key Packages

- **`packages/toolbar`**: Browser-based UI component
- **`packages/extension`**: VSCode extension with deep IDE integration
- **`packages/shared`**: Common utilities, types, and schemas
- **`packages/agents`**: Agent adapters for different AI tools

## 🎯 Contribution Areas

### 🐛 Bug Fixes

1. **Check Existing Issues**: Look for existing bug reports
2. **Reproduce the Bug**: Create a minimal reproduction case
3. **Write a Test**: Add a failing test that demonstrates the bug
4. **Fix the Bug**: Implement the fix
5. **Verify the Fix**: Ensure the test passes and no regressions

### ✨ New Features

1. **Discuss First**: Open an issue to discuss the feature
2. **Check TaskMaster**: See if there's already a related task
3. **Design the API**: Consider the public API and integration points
4. **Implement with Tests**: Write comprehensive tests
5. **Update Documentation**: Add or update relevant documentation

### 📚 Documentation

- **README Updates**: Improve setup, usage, or examples
- **Code Comments**: Add helpful comments to complex code
- **API Documentation**: Document public APIs and interfaces
- **Guides**: Create tutorials or how-to guides

### 🧪 Testing

- **Unit Tests**: Test individual functions and components
- **Integration Tests**: Test component interactions
- **E2E Tests**: Test complete user workflows
- **Performance Tests**: Test performance-critical paths

## 📝 Coding Standards

### TypeScript Guidelines

```typescript
// ✅ Use explicit types for public APIs
export interface ThreadContext {
  id: string;
  title: string;
  messages: Message[];
  metadata: ThreadMetadata;
}

// ✅ Use const assertions for immutable data
const SUPPORTED_AGENTS = ['cursor', 'windsurf'] as const;

// ✅ Use Zod schemas for runtime validation
import { z } from 'zod';

const MessageSchema = z.object({
  id: z.string(),
  content: z.string(),
  timestamp: z.number(),
  agent: z.enum(['cursor', 'windsurf', 'user']),
});
```

### React/Preact Components

```typescript
// ✅ Use functional components with hooks
import { useState, useEffect } from 'preact/hooks';

export function ThreadList({ threads }: { threads: Thread[] }) {
  const [filter, setFilter] = useState('');
  
  const filteredThreads = threads.filter(thread =>
    thread.title.toLowerCase().includes(filter.toLowerCase())
  );
  
  return (
    <div className="thread-list">
      <input
        type="text"
        placeholder="Filter threads..."
        value={filter}
        onInput={(e) => setFilter(e.currentTarget.value)}
      />
      {filteredThreads.map(thread => (
        <ThreadItem key={thread.id} thread={thread} />
      ))}
    </div>
  );
}
```

### Error Handling

```typescript
// ✅ Use Result types for error handling
type Result<T, E = Error> = 
  | { success: true; data: T }
  | { success: false; error: E };

export async function captureContext(selection: Selection): Promise<Result<ThreadContext>> {
  try {
    const context = await processSelection(selection);
    return { success: true, data: context };
  } catch (error) {
    return { 
      success: false, 
      error: error instanceof Error ? error : new Error('Unknown error') 
    };
  }
}
```

### Testing Patterns

```typescript
// ✅ Use descriptive test names
describe('ThreadStore', () => {
  it('should add a new thread with generated ID', () => {
    const store = new ThreadStore();
    const thread = store.addThread({ title: 'Test Thread' });
    
    expect(thread.id).toBeDefined();
    expect(thread.title).toBe('Test Thread');
    expect(store.getThreads()).toHaveLength(1);
  });

  it('should throw error when adding thread with duplicate ID', () => {
    const store = new ThreadStore();
    const existingThread = store.addThread({ title: 'Existing' });
    
    expect(() => {
      store.addThread({ id: existingThread.id, title: 'Duplicate' });
    }).toThrow('Thread with ID already exists');
  });
});
```

## 🧪 Testing Guidelines

### Unit Tests

- **Location**: Co-located with source files (`*.test.ts`)
- **Framework**: Vitest with @testing-library
- **Coverage**: Aim for 80%+ coverage on new code

```bash
# Run unit tests
pnpm test

# Run with coverage
pnpm test:coverage

# Run in watch mode
pnpm test:watch
```

### Integration Tests

- **Location**: `tests/integration/`
- **Purpose**: Test package interactions and APIs
- **Mocking**: Use MSW for HTTP mocking, Vitest spies for functions

### End-to-End Tests

- **Location**: `tests/e2e/`
- **Framework**: Playwright
- **Browsers**: Chromium, Firefox, WebKit

```bash
# Run E2E tests
pnpm test:e2e

# Run E2E tests in UI mode
pnpm test:e2e --ui

# Run specific test file
pnpm test:e2e tests/e2e/thread-management.spec.ts
```

## 🔧 Development Commands

```bash
# Development
pnpm dev                    # Start all development servers
pnpm dev --filter=toolbar   # Start toolbar development only
pnpm dev --filter=extension # Start extension development only

# Building
pnpm build                  # Build all packages
pnpm build --filter=toolbar # Build toolbar package only

# Testing
pnpm test                   # Run unit tests
pnpm test:e2e              # Run end-to-end tests
pnpm test:coverage         # Run tests with coverage

# Quality
pnpm lint                   # Run ESLint
pnpm lint:fix              # Fix linting issues
pnpm type-check            # Run TypeScript checks
pnpm format                # Format code with Prettier

# TaskMaster
pnpm task-master list      # View current tasks
pnpm task-master next      # Get next task to work on
```

## 📤 Pull Request Process

### Before Submitting

1. **Rebase on Latest Main**
   ```bash
   git fetch origin
   git rebase origin/main
   ```

2. **Run Quality Checks**
   ```bash
   pnpm lint
   pnpm type-check
   pnpm test
   pnpm test:e2e
   ```

3. **Update Documentation**
   - Update README if needed
   - Add or update relevant documentation
   - Update CHANGELOG if significant changes

### Pull Request Template

Use this template for your pull request description:

```markdown
## Description
Brief description of changes and motivation.

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Related Issues
Closes #issue_number

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] E2E tests pass
- [ ] Manual testing completed

## Screenshots (if applicable)
Add screenshots for UI changes.

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] Tests added/updated
- [ ] No breaking changes (or clearly documented)
```

### Review Process

1. **Automated Checks**: CI will run tests and quality checks
2. **Code Review**: Maintainers will review your code
3. **Feedback**: Address any requested changes
4. **Merge**: Once approved, we'll merge your PR

## 🆘 Getting Help

### Resources

- **Documentation**: Check the `docs/` directory
- **API Reference**: Look at TypeScript definitions in `packages/shared/types/`
- **Examples**: See the demo app in `apps/demo/`
- **TaskMaster**: Use `pnpm task-master next` to find beginner-friendly tasks

### Communication

- **GitHub Issues**: For bugs and feature requests
- **GitHub Discussions**: For questions and community discussions
- **Pull Request Comments**: For code-specific discussions

### Common Issues

**Build Errors**
```bash
# Clear node_modules and reinstall
rm -rf node_modules
pnpm install

# Clear build cache
pnpm clean
pnpm build
```

**Test Failures**
```bash
# Run tests in verbose mode
pnpm test --verbose

# Run specific test file
pnpm test path/to/test.spec.ts
```

**Extension Development**
- Make sure VSCode is version 1.85+
- Press `F5` to launch extension development host
- Check the Debug Console for extension logs

## 🏆 Recognition

Contributors will be recognized in:
- **README.md**: Contributors section
- **CHANGELOG.md**: Credit for significant contributions
- **GitHub**: Contributor badge and statistics

## 📄 License

By contributing to ThreadBridge, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to ThreadBridge! 🎉
