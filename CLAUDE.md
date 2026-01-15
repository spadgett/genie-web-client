# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Basic Development
- `yarn install` - Install dependencies
- `yarn start` - Start plugin development server (port 9001)
- `yarn start-console` - Start OpenShift console with plugin (port 9000)
- `yarn build` - Production build
- `yarn build-dev` - Development build

### Testing
- `yarn test` - Run unit tests (Jest)
- `yarn test:watch` - Run tests in watch mode
- `yarn test:coverage` - Run tests with coverage report
- `yarn test-cypress` - Run Cypress integration tests (interactive)
- `yarn test-cypress-headless` - Run Cypress tests (headless)

### Code Quality
- `yarn lint` - Lint TypeScript/JavaScript and CSS files (with auto-fix)
- TypeScript checking is handled during build process

### Special Commands
- `yarn i18n` - Build internationalization files

## Architecture Overview

### OpenShift Console Plugin
This is an OpenShift Console dynamic plugin built with:
- **Framework**: React 17 with TypeScript
- **UI Library**: PatternFly 6 (components, icons, and ChatBot)
- **Bundling**: Webpack with OpenShift Console Plugin SDK
- **State Management**: Red Hat AI state management libraries

### AI Integration
- **Backend**: Communicates with OLS (OpenShift Lightspeed Service) via `/api/proxy/plugin/genie-web-client/ols/`
- **Client**: Custom `OLSClient` in `src/components/utils/olsClient.ts` implements streaming and non-streaming AI interactions
- **State**: Uses `@redhat-cloud-services/ai-client-state` and `@redhat-cloud-services/ai-react-state` for AI state management

### Project Structure
```
src/
├── components/           # React components organized by feature
│   ├── Chat/            # Chat interface and messaging
│   ├── layout/          # Main layout and drawer management
│   ├── theme/           # Theme switching functionality
│   ├── onboarding/      # User onboarding modals
│   └── utils/           # Utilities (AI client, state manager)
├── hooks/               # Custom React hooks, primarily AI state
└── types/               # TypeScript type definitions
```

### Key Components
- **Routes.tsx**: Main routing using react-router-dom-v5-compat
- **GeniePage.tsx**: Root component with providers (Theme, AI, ChatBar, Drawer)
- **Layout.tsx**: Main layout with drawer functionality
- **Chat components**: Handle conversation UI and messaging
- **AIProvider**: Wraps app with AI state management
- **OLSClient**: Custom client for backend communication with CSRF token handling

### Development Setup Requirements
The application requires both frontend and backend services:
1. **obs-mcp server**: Monitoring/metrics integration (port 9100)
2. **lightspeed-stack**: AI backend service (port 8080)
3. **Plugin dev server**: Frontend development (port 9001)
4. **OpenShift console**: Console with plugin loaded (port 9000)

Access the application at: http://localhost:9000/genie

## Code Conventions

### General
- Use camelCase for functions/variables, PascalCase for components
- Extract reusable logic into custom hooks/functions
- Handle loading and error states with user-friendly messages
- Use ternary operators instead of if/else when appropriate
- Prefer const > let > var
- Organize imports: external libraries → internal modules → relative imports

### React/TypeScript
- Use functional components with hooks
- Prefer `React.FC` for component type annotations
- Use named imports for React hooks (`useEffect`, `useState`)
- Boolean props should be prefixed with `is`, `has`, `can`, or `should`
- Use `useMemo` for expensive calculations and context values
- Use `useCallback` for functions passed to children
- Define explicit return types for custom hooks
- Prefer named exports over default exports for components
- Use barrel exports (index.ts) to organize component exports
- Always validate context usage within providers
- Prefer `interface` over `type` for object shapes
- Use `import type` for type-only imports
- Avoid `any` type - use `unknown` with type guards instead

### Testing
- Place test files alongside components (`.test.tsx`)
- Use `describe` for component names, nested blocks for related tests
- Follow Arrange-Act-Assert pattern
- Test behavior, not implementation details
- Import `render` and `screen` from `src/unitTestUtils`
- Query priority: `getByRole` → `getByLabelText` → `getByText` → `getByTestId`
- Use `findBy*` for async elements, avoid `waitFor` when possible
- Use `checkAccessibility` for accessibility testing
- Test error states, loading states, and edge cases

### File Organization
- Single test file: co-located (e.g., `Component.test.tsx`)
- Multiple test files: use `__tests__/` directory
- Mock files go in `__mocks__/` at project root
- Use PatternFly variables/utility classes instead of custom CSS when possible