# CLAUDE.md
This file provides guidance to Claude Code (claude.ai/code) when working with
code in the KRIN XXI repository.
## Project Overview
KRIN XXI is a workflow automation platform written in TypeScript, using a monorepo
structure managed by pnpm workspaces. It consists of a Node.js backend, Vue.js
frontend, and extensible node-based workflow engine.
## General Guidelines
- Always use pnpm
- We use Linear as a ticket tracking system
- We use Posthog for feature flags
- When starting to work on a new ticket – create a new branch from fresh
  master with the name specified in Linear ticket
- When creating a new branch for a ticket in Linear - use the branch name
  suggested by linear
- Use mermaid diagrams in MD files when you need to visualise something
## Essential Commands
### Building
Use `pnpm build` to build all packages. ALWAYS redirect the output of the
build command to a file:
```bash
pnpm build > build.log 2>&1
```
You can inspect the last few lines of the build log file to check for errors:
```bash
tail -n 20 build.log
```
### Testing
- `pnpm test` - Run all tests
- `pnpm test:affected` - Runs tests based on what has changed since the last
  commit
Running a particular test file requires going to the directory of that test
and running: `pnpm test <test-file>`.
When changing directories, use `pushd` to navigate into the directory and
`popd` to return to the previous directory. When in doubt, use `pwd` to check
your current directory.
### Code Quality
- `pnpm lint` - Lint code
- `pnpm typecheck` - Run type checks
Always run lint and typecheck before committing code to ensure quality.
Execute these commands from within the specific package directory you're
working on (e.g., `cd packages/cli && pnpm lint`). Run the full repository
check only when preparing the final PR. When your changes affect type
definitions, interfaces in `@KRIN XXI/api-types`, or cross-package dependencies,
build the system before running lint and typecheck.
## Architecture Overview
**Monorepo Structure:** pnpm workspaces with Turbo build orchestration
### Package Structure
The monorepo is organized into these key packages:
- **`packages/@KRIN XXI/api-types`**: Shared TypeScript interfaces between frontend and backend
- **`packages/workflow`**: Core workflow interfaces and types
- **`packages/core`**: Workflow execution engine
- **`packages/cli`**: Express server, REST API, and CLI commands
- **`packages/editor-ui`**: Vue 3 frontend application
- **`packages/@KRIN XXI/i18n`**: Internationalization for UI text
- **`packages/nodes-base`**: Built-in nodes for integrations
- **`packages/@KRIN XXI/nodes-langchain`**: AI/LangChain nodes
- **`@KRIN XXI/design-system`**: Vue component library for UI consistency
- **`@KRIN XXI/config`**: Centralized configuration management
## Technology Stack
- **Frontend:** Vue 3 + TypeScript + Vite + Pinia + Storybook UI Library
- **Backend:** Node.js + TypeScript + Express + TypeORM
- **Testing:** Jest (unit) + Playwright (E2E)
- **Database:** TypeORM with SQLite/PostgreSQL/MySQL support
- **Code Quality:** Biome (for formatting) + ESLint + lefthook git hooks
### Key Architectural Patterns
1. **Dependency Injection**: Uses `@KRIN XXI/di` for IoC container
