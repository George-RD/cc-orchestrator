# Context Management Guidelines

## When to Use /compact
- After completing major task groups (3+ tasks)
- Before switching between phases (e.g., dev → testing)
- When approaching context limits
- After major refactoring sessions

## Context Budget Guidelines
- **Orchestrator**: Keep minimal, continuous context
- **Specialists**: Start fresh with shared ethos reference
- **PRDs**: Load only when actively working on tasks
- **Tasks**: Load individual tasks as needed, not entire registry

## Preservation Strategy
```
Essential to preserve:
- Current task being worked on
- Confidence levels and concerns
- Key architectural decisions
- Critical blockers or dependencies

Safe to reset:
- Detailed implementation discussions
- Code review comments
- Historical task logs
- Verbose error messages
```

## Recovery Protocol
If context is lost:
1. Check current git branch and uncommitted changes
2. Read active task work_log for progress
3. Review task acceptance criteria
4. Scan recent commit messages for context