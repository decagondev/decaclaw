# Decaclaw Development Plan: EPICs, User Stories, PRs, Commits, and Subtasks (Updated)

This updated document incorporates EPIC 0 as a foundational prerequisite, establishing rules for using Cursor CLI (the AI agent) to enforce SOLID Principles (Single Responsibility, Open-Closed, Liskov Substitution, Interface Segregation, Dependency Inversion) and a modular design throughout the codebase. It also mandates a standardized Git workflow for every feature: 
- Checkout a new feature branch (e.g., `git checkout -b feature/[feature-name]`).
- Make atomic commits for the feature implementation.
- Add tests (unit, integration) for the feature.
- Run tests (`npm test` or equivalent).
- Lint code (`npm run lint`) and check types (`tsc --noEmit`).
- Fix any errors until all checks pass.
- Create a PR, review, and merge back to main only when passing.

This workflow applies to **each feature** (i.e., each PR under User Stories). The plan assumes a TypeScript/Node.js setup with tools like Jest for testing, ESLint for linting, and TypeScript for type checking. Cursor CLI will be used AI-natively to assist in refactoring for SOLID/modularity (e.g., via prompts like "Refactor this module to follow SOLID"). Total estimated effort remains 4-6 weeks, with EPIC 0 adding 1 week upfront.

## EPIC 0: Foundational Rules and Workflow Setup
This EPIC establishes core rules for development using Cursor CLI to enforce SOLID Principles and modular design, while integrating the required Git workflow. It sets up the project structure, tools, and guidelines before tackling other EPICs.

### User Story 0.1: As a developer, I want Cursor CLI rules integrated to enforce SOLID Principles and modular design so that the codebase remains maintainable, scalable, and extensible.
- **PR 0.1.1: Setup Cursor CLI for SOLID and Modular Refactoring**
  - Commit 1: Install Cursor CLI and add setup script for AI-assisted refactoring.
  - Commit 2: Define SOLID rules in docs/SOLID_GUIDELINES.md (e.g., each module has single responsibility; use interfaces for dependency inversion).
  - Commit 3: Create modular structure (e.g., src/modules/ai, src/modules/messaging, src/modules/core with clear interfaces).
  - Subtasks:
    - Use Cursor CLI prompt: "Analyze codebase for SOLID violations and suggest fixes."
    - Refactor initial files (e.g., src/index.ts) to modular components.
    - Add .cursor/config for predefined prompts (e.g., "Ensure Open-Closed Principle").
    - Test: Run Cursor CLI on a sample module and apply changes.

- **PR 0.1.2: Implement Modular Design Patterns**
  - Commit 1: Break down monolithic parts into modules (e.g., separate AI invocation, container management).
  - Commit 2: Apply Dependency Inversion (e.g., inject Cursor CLI via interfaces).
  - Commit 3: Document modular design in docs/ARCHITECTURE.md.
  - Subtasks:
    - Use Cursor CLI for code generation: "Generate interface for AIProvider following SOLID."
    - Ensure Liskov Substitution: Subclasses (e.g., CursorAgent) can replace base without issues.
    - Integration test: Verify modules interact correctly.

### User Story 0.2: As a developer, I want a standardized Git workflow enforced for each feature so that code quality is maintained through branching, testing, linting, and merging.
- **PR 0.2.1: Configure Git Workflow Tools and Scripts**
  - Commit 1: Add pre-commit hooks for linting and type checking (using husky).
  - Commit 2: Setup testing framework (Jest) and linting (ESLint with TypeScript rules).
  - Commit 3: Create workflow script (e.g., scripts/feature-workflow.sh) to automate: branch checkout, commit, test, lint, PR creation.
  - Subtasks:
    - Document workflow in docs/GIT_WORKFLOW.md: "For each feature: git checkout -b feature/[name], commit changes, add tests, npm test, npm run lint, tsc --noEmit, then PR and merge."
    - Use Cursor CLI to generate hook scripts: "Create husky pre-commit for SOLID checks."
    - Test workflow: Simulate on a dummy feature.

- **PR 0.2.2: Enforce Workflow in Project Setup**
  - Commit 1: Update package.json with scripts (e.g., "test", "lint", "typecheck").
  - Commit 2: Add CI/CD config (e.g., GitHub Actions) to run workflow on PRs.
  - Commit 3: Integrate Cursor CLI into workflow for auto-refactoring before merge.
  - Subtasks:
    - Ensure all checks pass before merge: Fail PR if tests/lint/types fail.
    - Example: For each PR below, developers must follow: Branch -> Commits -> Tests -> Run checks -> Merge.

## EPIC 1: AI Provider Transition
(Workflow: Apply Git workflow to each PR – branch, commits, tests, lint/types, merge.)

### User Story 1.1: As a developer, I want to integrate Cursor CLI as the primary AI agent so that tasks like code generation and execution are handled securely in containers.
- **PR 1.1.1: Integrate Cursor CLI Basics**
  - Commit 1: Install and configure Cursor CLI dependencies.
  - Commit 2: Replace Claude SDK imports with Cursor CLI calls.
  - Commit 3: Add basic agent invocation in src/index.ts.
  - Subtasks:
    - Branch: git checkout -b feature/integrate-cursor-basics
    - Tests: Add unit tests for agent invocation.
    - Run: npm test; npm run lint; tsc --noEmit
    - Research Cursor CLI API for headless mode.
    - Review for SOLID: Use Cursor CLI to check modularity.

- **PR 1.1.2: Adapt Container Runners for Cursor CLI**
  - Commit 1: Modify src/container-runner.ts to spawn Cursor CLI processes.
  - Commit 2: Handle IPC between main process and Cursor agents.
  - Commit 3: Add error handling for Cursor failures.
  - Subtasks:
    - Branch: git checkout -b feature/adapt-container-cursor
    - Tests: Integration tests for container execution.
    - Run: npm test; npm run lint; tsc --noEmit
    - Test in Apple Container and Docker.
    - Document in docs/CURSOR.md.

### User Story 1.2: As a user, I want support for multiple models via Cursor so that I can switch between GPT-5, Claude 4, etc., for optimal performance.
- **PR 1.2.1: Implement Model Selection Flags**
  - Commit 1: Add command-line flags for model selection in src/index.ts.
  - Commit 2: Integrate flags into Cursor CLI calls.
  - Commit 3: Update memory files (DECACLAW.md) to store model preferences.
  - Subtasks:
    - Branch: git checkout -b feature/model-selection
    - Tests: Unit tests for flag parsing.
    - Run: npm test; npm run lint; tsc --noEmit
    - List supported models in README.

- **PR 1.2.2: AI-Native Customization with Cursor**
  - Commit 1: Replace Claude-guided edits with Cursor CLI prompts.
  - Commit 2: Implement /customize command to invoke Cursor agent.
  - Commit 3: Add examples in docs/customization.md.
  - Subtasks:
    - Branch: git checkout -b feature/ai-customization
    - Tests: Acceptance tests for customization flow.
    - Run: npm test; npm run lint; tsc --noEmit
    - Create sample prompts.

## EPIC 2: Messaging Platform Transition
(Workflow: Apply Git workflow to each PR – branch, commits, tests, lint/types, merge.)

### User Story 2.1: As a user, I want Telegram bot integration for messaging so that I can interact via a secure, open platform.
- **PR 2.1.1: Setup Telegram Bot API**
  - Commit 1: Add node-telegram-bot-api dependency.
  - Commit 2: Implement bot token authentication in setup script.
  - Commit 3: Replace WhatsApp connection logic in src/index.ts.
  - Subtasks:
    - Branch: git checkout -b feature/telegram-setup
    - Tests: Unit tests for auth.
    - Run: npm test; npm run lint; tsc --noEmit
    - Update setup in README.

- **PR 2.1.2: Message Sending and Receiving**
  - Commit 1: Adapt message queue (src/group-queue.ts) for Telegram events.
  - Commit 2: Implement send/reply functions for Telegram.
  - Commit 3: Add group join/leave handling.
  - Subtasks:
    - Branch: git checkout -b feature/telegram-messaging
    - Tests: Integration tests for message flow.
    - Run: npm test; npm run lint; tsc --noEmit
    - Handle rate limits.

### User Story 2.2: As an admin, I want trigger-based activation (@Deca) so that the assistant responds only when invoked.
- **PR 2.2.1: Customize Triggers for Telegram**
  - Commit 1: Update trigger logic from @Andy to @Deca.
  - Commit 2: Make triggers configurable via Cursor CLI.
  - Commit 3: Add per-group trigger overrides.
  - Subtasks:
    - Branch: git checkout -b feature/telegram-triggers
    - Tests: Unit tests for trigger matching.
    - Run: npm test; npm run lint; tsc --noEmit
    - Document examples.

## EPIC 3: Core Feature Preservation and Adaptation
(Workflow: Apply Git workflow to each PR – branch, commits, tests, lint/types, merge.)

### User Story 3.1: As a user, I want per-group isolation and memory so that contexts remain secure and separate.
- **PR 3.1.1: Adapt Group Context Management**
  - Commit 1: Rename memory files from NANOCRAWL.md to DECACLAW.md.
  - Commit 2: Ensure filesystem isolation in containers with Cursor.
  - Commit 3: Update SQLite schema for groups if needed.
  - Subtasks:
    - Branch: git checkout -b feature/group-context
    - Tests: Isolation tests for parallel groups.
    - Run: npm test; npm run lint; tsc --noEmit
    - Migrate sample data.

- **PR 3.1.2: Preserve Agent Swarms**
  - Commit 1: Adapt swarm logic to use Cursor CLI agents.
  - Commit 2: Implement collaborative task handling.
  - Commit 3: Add swarm examples in docs.
  - Subtasks:
    - Branch: git checkout -b feature/agent-swarms
    - Tests: Integration tests for swarms.
    - Run: npm test; npm run lint; tsc --noEmit
    - Define "deca" enhancement.

### User Story 3.2: As a user, I want scheduled tasks so that recurring jobs run automatically via Telegram.
- **PR 3.2.1: Update Task Scheduler**
  - Commit 1: Retain src/task-scheduler.ts logic.
  - Commit 2: Integrate with Telegram for notifications.
  - Commit 3: Add admin commands (e.g., list tasks) from main channel.
  - Subtasks:
    - Branch: git checkout -b feature/task-scheduler
    - Tests: Scheduling tests.
    - Run: npm test; npm run lint; tsc --noEmit
    - Handle timezones.

## EPIC 4: Branding and Documentation
(Workflow: Apply Git workflow to each PR – branch, commits, tests, lint/types, merge.)

### User Story 4.1: As a contributor, I want updated branding so that the project reflects the new name and philosophy.
- **PR 4.1.1: Global Rename and Rebrand**
  - Commit 1: Rename files, variables, and references (nanoclaw -> decaclaw).
  - Commit 2: Update triggers and logos if any.
  - Commit 3: Emphasize "deca" in README (e.g., 10x scalability).
  - Subtasks:
    - Branch: git checkout -b feature/rebranding
    - Tests: N/A (docs), but lint code changes.
    - Run: npm run lint; tsc --noEmit
    - Search/replace codebase.

- **PR 4.1.2: Comprehensive Documentation**
  - Commit 1: Update README with Decaclaw overview.
  - Commit 2: Add docs/SECURITY.md and docs/customization.md.
  - Commit 3: Include migration guide from Nanoclaw.
  - Subtasks:
    - Branch: git checkout -b feature/documentation
    - Tests: N/A, but validate links.
    - Run: npm run lint (if applicable); tsc --noEmit
    - Add diagrams.

## EPIC 5: Setup, Customization, and Extensibility
(Workflow: Apply Git workflow to each PR – branch, commits, tests, lint/types, merge.)

### User Story 5.1: As a new user, I want an easy setup process so that I can get started quickly.
- **PR 5.1.1: AI-Native Setup Script**
  - Commit 1: Use Cursor CLI to automate dependency install.
  - Commit 2: Implement /setup command for Telegram bot config.
  - Commit 3: Handle container setup (Apple/Docker).
  - Subtasks:
    - Branch: git checkout -b feature/setup-script
    - Tests: End-to-end setup tests.
    - Run: npm test; npm run lint; tsc --noEmit
    - Test on macOS/Linux.

### User Story 5.2: As a developer, I want skill-based extensibility so that I can add features without core changes.
- **PR 5.2.1: Adapt Skills Directory**
  - Commit 1: Create .deca/skills structure.
  - Commit 2: Implement /add-skill command via Cursor.
  - Commit 3: Add example skills (e.g., /add-gmail, /add-whatsapp).
  - Subtasks:
    - Branch: git checkout -b feature/skills-extensibility
    - Tests: Tests for adding skills.
    - Run: npm test; npm run lint; tsc --noEmit
    - Document guidelines.

## EPIC 6: Security, Testing, and Launch
(Workflow: Apply Git workflow to each PR – branch, commits, tests, lint/types, merge.)

### User Story 6.1: As a user, I want secure operations so that my data remains protected.
- **PR 6.1.1: Security Enhancements**
  - Commit 1: Review container mounts for minimal access.
  - Commit 2: Secure API keys (Cursor, Telegram).
  - Commit 3: Update docs/SECURITY.md.
  - Subtasks:
    - Branch: git checkout -b feature/security-enhancements
    - Tests: Security scans.
    - Run: npm test; npm run lint; tsc --noEmit
    - Run npm audit.

### User Story 6.2: As a developer, I want thorough testing so that the transition is bug-free.
- **PR 6.2.1: Implement Testing Suite**
  - Commit 1: Add unit tests for core modules.
  - Commit 2: Integration tests for full flows.
  - Commit 3: End-to-end tests with Telegram simulation.
  - Subtasks:
    - Branch: git checkout -b feature/testing-suite
    - Tests: Achieve 80% coverage.
    - Run: npm test; npm run lint; tsc --noEmit

- **PR 6.2.2: Launch Preparation**
  - Commit 1: Final code review.
  - Commit 2: Update timeline in PRD.
  - Commit 3: Push to new repo and announce.
  - Subtasks:
    - Branch: git checkout -b feature/launch-prep
    - Tests: Full suite run.
    - Run: npm test; npm run lint; tsc --noEmit
    - Create release notes.
