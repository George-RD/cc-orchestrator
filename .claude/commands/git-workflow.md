# Git Worktree Workflow

## Architecture Overview
Each specialist operates in dedicated git worktrees, enabling true parallel development with session recovery.

## Worktree Structure
```
project-root/               # Main branch (orchestrator)
├── .git/
├── worktrees/
│   ├── backend-work/       # Backend specialist worktree
│   ├── frontend-work/      # Frontend specialist worktree  
│   ├── qa-work/           # QA specialist worktree
│   └── docs-work/         # Docs specialist worktree
```

## Workflow Patterns

### Task Assignment (Orchestrator)
```bash
# 1. Create/update specialist worktree
git worktree add worktrees/backend-work backend-work || git worktree repair

# 2. Ensure branch is up-to-date with main
cd worktrees/backend-work
git fetch origin
git reset --hard origin/main

# 3. Create task-specific commit for tracking
git commit --allow-empty -m "Start task-{id}: {title}"

# 4. Delegate to specialist with worktree context
```

### Specialist Development
```bash
# Work in assigned worktree
cd worktrees/{specialist}-work

# Regular commits during development
git add .
git commit -m "Progress on task-{id}: {specific_change}"

# Update task JSON with progress
# Confidence assessment at milestones
```

### Task Completion (Specialist)
```bash
# Final commit with confidence level
git add .
git commit -m "Complete task-{id}: {title} (confidence: {level})"

# Update task JSON status and handoff
# Return to orchestrator for integration
```

### Integration (Orchestrator)
```bash
# Review specialist work
cd worktrees/{specialist}-work
git log --oneline origin/main..HEAD

# Create PR-style merge with conflict resolution
cd project-root
git checkout main
git merge worktrees/{specialist}-work/{specialist}-work --no-ff -m "Merge task-{id}: {title}"

# Handle conflicts if needed
# Run integration tests
# Clean up or preserve branch for future work
```

## Session Recovery Protocol

### Mid-Task Recovery
```bash
# Orchestrator checks active tasks
ls .orchestrator/tasks/active/

# For each active task, check corresponding worktree
cd worktrees/{specialist}-work
git log --oneline -5  # Recent commits show progress

# Resume from last commit + task JSON state
```

### Conflict Resolution
```bash
# When merge conflicts occur
git status  # Show conflicted files
git diff    # Review conflicts

# Orchestrator resolves with full context
# Commits resolution with detailed message
git commit -m "Resolve conflicts in task-{id}: {resolution_notes}"
```

## Branch Naming Convention
- `backend-work` - Backend specialist development
- `frontend-work` - Frontend specialist development  
- `qa-work` - QA specialist development
- `docs-work` - Documentation specialist development
- `feature/{task-id}` - Optional task-specific branches for complex features

## Commit Message Format
```
{action} task-{id}: {description} (confidence: {level})

Examples:
- "Start task-123: User authentication system"
- "Progress on task-123: Add JWT token validation"  
- "Complete task-123: User auth system (confidence: high)"
- "Merge task-123: User authentication system"
```

## Integration Points

### With Task JSON
```json
{
  "id": "task-123",
  "git_branch": "backend-work",
  "git_worktree": "worktrees/backend-work",
  "last_commit": "abc123def",
  "commits": [
    {"hash": "abc123", "message": "Start task-123", "timestamp": "..."},
    {"hash": "def456", "message": "Progress: JWT setup", "timestamp": "..."}
  ]
}
```

### With Confidence System
- **High confidence**: Auto-merge after tests pass
- **Medium confidence**: Create PR for review with notes
- **Low confidence**: Hold for human review with concerns

## Commands Integration
- `/git-status` - Show all worktree statuses and active tasks
- `/git-sync` - Update all worktrees from main branch
- `/git-merge task-{id}` - Merge specific completed task