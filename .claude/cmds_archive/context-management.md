# Context Management Guidelines

## Auto-Compact Integration
Claude Code's auto-compact handles context optimization automatically. The orchestrator manages context naturally through delegation patterns.

## Context Budget Guidelines
- **Orchestrator**: Keep minimal, continuous context
- **Specialists**: Start fresh with shared ethos reference
- **PRDs**: Load only when actively working on tasks
- **Tasks**: Load individual tasks as needed, not entire registry

## Recovery Protocol
If context is lost:
1. Check current git branch and uncommitted changes
2. Read active task work_log for progress
3. Review task acceptance criteria
4. Scan recent commit messages for context

## Natural Flow Strategy
The orchestrator maintains context through:
- Clear task delegation with essential background
- Confidence-based handoffs preserve key decisions
- Shared ethos provides consistent foundation
- Task JSON maintains resumability state