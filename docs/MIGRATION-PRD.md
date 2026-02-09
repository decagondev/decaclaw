# Decaclaw Product Requirements Document (PRD)

## 1. Introduction

### 1.1 Project Overview
Decaclaw is a rebranded and evolved version of the Nanoclaw project, transforming it into a lightweight, secure, and customizable AI assistant. The core philosophy remains intact: a minimal, understandable codebase that prioritizes security through OS-level isolation, simplicity, and direct integration with an AI agent framework. Decaclaw shifts from Anthropic's Claude Code and Agents SDK to Cursor CLI's agent capabilities, while replacing WhatsApp integration with Telegram for messaging. This transition maintains the general design—single process architecture, per-group memory, scheduled jobs, container isolation, and agent swarms—but adapts to a new LLM provider (Cursor's supported models, such as GPT-5, Claude 4, or others) and messaging platform.

The project is designed for personal use, emphasizing user control, transparency, and AI-native customization. It will continue to support agent swarms for collaborative task handling, with skills-based extensibility to avoid core bloat.

### 1.2 Objectives
- **Transition to Cursor CLI**: Replace Claude Code with Cursor CLI agents for AI-driven tasks, leveraging its terminal-based, agentic capabilities for code generation, execution, and interaction.
- **Switch to Telegram**: Replace WhatsApp as the primary I/O channel with Telegram, using a bot API for seamless messaging.
- **Maintain Core Design**: Preserve isolation via containers (Apple Container on macOS or Docker), per-group context, scheduling, and minimalism.
- **Branding as Decaclaw**: Update all references, documentation, and naming to reflect the new brand, evoking a "deca" (tenfold) enhancement in scalability or power while keeping the "claw" motif.
- **Enhance Customizability**: Ensure AI-native workflows for setup, debugging, and modifications, now powered by Cursor CLI.
- **Security and Performance**: Retain sandboxing and add any Cursor-specific optimizations.

### 1.3 Target Audience
- Individual developers or users seeking a personal AI assistant.
- Those familiar with terminal-based tools, containers, and messaging apps.
- Users transitioning from Nanoclaw or similar projects, prioritizing privacy and control.

## 2. Scope

### 2.1 In Scope
- Refactor core codebase to integrate Cursor CLI instead of Claude Agents SDK.
- Implement Telegram bot integration for send/receive messaging.
- Update container runners to execute Cursor CLI agents in isolated environments.
- Preserve and adapt features: per-group memory (via files like DECACLAW.md), scheduled tasks, agent swarms, web access.
- AI-native setup and customization (e.g., use Cursor CLI for natural language-guided changes).
- Documentation updates, including README, setup instructions, and skills.
- Basic testing for macOS and Linux environments.
- Rebranding: Rename repository to decaclaw, update triggers (e.g., @Deca instead of @Andy), and file names where appropriate.

### 2.2 Out of Scope
- Support for additional platforms (e.g., Windows) unless added via skills.
- New features beyond the transition (e.g., no additional channels unless via skills).
- Advanced Cursor CLI modes unless they directly replace Claude functionalities.
- Performance benchmarking against Nanoclaw.
- Mobile or web UI; remains terminal/container-based.

## 3. Features

### 3.1 Core Features (Retained from Nanoclaw)
- **Isolated Group Context**: Each Telegram chat/group maintains:
  - Dedicated DECACLAW.md memory file.
  - Isolated filesystem.
  - Runs in its own container sandbox.
- **Main Channel**: Private self-chat for admin controls (e.g., list tasks, join groups).
- **Scheduled Tasks**: Recurring jobs that trigger the AI agent and send responses via Telegram.
- **Web Access**: Agent can search and fetch content.
- **Container Isolation**: Use Apple Container (macOS) or Docker (macOS/Linux) for agent execution.
- **Agent Swarms**: Support for teams of specialized AI agents collaborating on tasks.
- **Skill-Based Extensibility**: Add functionalities (e.g., /add-gmail) via .deca/skills directory without core changes.

### 3.2 Key Changes and New Features
- **AI Provider Transition**:
  - Replace Claude Code/Agents SDK with Cursor CLI agent.
  - Use Cursor CLI in headless or interactive modes for tasks like code generation, terminal command execution, codebase search, and natural language processing.
  - Support multiple models via Cursor (e.g., GPT-5 as default, with flags for others like Claude 4).
  - Adapt AI-native customization: Instead of Claude-guided edits, use Cursor CLI prompts (e.g., "agent -p 'Change trigger to @Deca'").
- **Messaging Platform Transition**:
  - Replace WhatsApp (baileys library) with Telegram (using node-telegram-bot-api or telegraf library).
  - Support bot token authentication during setup.
  - Handle message polling, sending, and group management similarly to WhatsApp.
  - Trigger assistant with @Deca (customizable).
- **Usage Examples** (Adapted):
  - "@Deca send an overview of the sales pipeline every weekday morning at 9am (has access to my Obsidian vault folder)"
  - "@Deca review the git history for the past week each Friday and update the README if there's drift"
  - From main channel: "@Deca list all scheduled tasks across groups"
- **Customization**:
  - No config files; modify code directly or via Cursor CLI (e.g., /customize command invokes Cursor agent).
  - Skills like /add-whatsapp for optional backward compatibility.

### 3.3 User Flows
- **Setup**: Clone repo, run 'cursor-agent' (or equivalent), then /setup. Cursor CLI handles dependencies, auth, container setup.
- **Interaction**: Send messages in Telegram; agent processes in container, responds.
- **Scheduling**: Define tasks in chat; stored in SQLite and executed periodically.
- **Debugging**: Use /debug to invoke Cursor CLI for issue diagnosis.

## 4. Technical Requirements

### 4.1 Architecture
- **High-Level Flow**:
  ```
  Telegram (bot API) --> SQLite --> Polling loop --> Container (Cursor CLI Agent) --> Response
  ```
  - Single Node.js process.
  - Per-group message queue with concurrency control.
  - IPC via filesystem for containers.
- **Key Components**:
  - src/index.ts: Main app – Telegram connection, message loop, IPC.
  - src/group-queue.ts: Queue management.
  - src/container-runner.ts: Spawns Cursor CLI agents in containers (adapt to run 'cursor-agent' commands).
  - src/task-scheduler.ts: Job scheduling.
  - src/db.ts: SQLite for messages, groups, sessions, state.
  - groups/*/DECACLAW.md: Per-group memory.

### 4.2 Dependencies
- **Runtime**: Node.js 20+.
- **AI**: Cursor CLI (installed via curl script during setup).
- **Messaging**: node-telegram-bot-api or telegraf.
- **Containerization**: Apple Container (macOS) or Docker.
- **Database**: SQLite.
- **Others**: Retain minimal deps; add any Cursor-specific if needed.

### 4.3 Setup Instructions
- Git clone https://github.com/[user]/decaclaw.git
- cd decaclaw
- Install Cursor CLI: curl https://cursor.com/install -fsSL | bash
- Run cursor-agent
- Invoke /setup: Cursor CLI automates dependency install, Telegram bot token setup, container config.

### 4.4 Development Environment
- macOS or Linux.
- TypeScript for core code.
- No additional build tools beyond tsconfig.json.

## 5. Customization and Extensibility
- **Philosophy**: Keep core small; extend via skills (e.g., .deca/skills/add-slack/SKILL.md).
- **AI-Native**: Use Cursor CLI for customizations like "Change the trigger word to @Bob".
- **Contributing**: Add skills, not features. RFS for new skills (e.g., /setup-windows).

## 6. Security
- **Isolation**: Agents run in containers with limited mounts.
- **Data Handling**: Per-group sandboxes; no shared state.
- **Auth**: Telegram bot token; Cursor API keys managed securely.
- Document in docs/SECURITY.md.

## 7. Branding and Documentation
- **Naming**: All references to "nanoclaw", "Claude", "Andy" updated to "decaclaw", "Cursor", "Deca".
- **README**: Update with Decaclaw overview, quick start, philosophy (emphasize "deca" for enhanced agent swarms or multi-model support).
- **License**: Retain MIT.
- **Community**: Link to relevant Discord or forums.

## 8. Risks and Mitigations
- **Cursor CLI Compatibility**: Test headless mode for container integration; fallback to interactive if needed.
- **Telegram Limits**: Handle rate limits in polling loop.
- **Model Differences**: Cursor supports Claude models, so minimal disruption; test prompts for equivalence.
- **Migration**: Provide a skill for Nanoclaw users to transition data.

## 9. Timeline and Milestones
- **Phase 1**: Refactor AI integration (2 weeks).
- **Phase 2**: Implement Telegram (1 week).
- **Phase 3**: Testing and documentation (1 week).
- **Launch**: Open-source as decaclaw repo.

This PRD serves as a blueprint for development, ensuring a smooth transition while preserving the essence of the original project.
