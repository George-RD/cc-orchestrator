# Git Worktree Workflow

## Architecture Overview
Two orchestration patterns: parallel development for code specialists in worktrees, sequential coordination for infrastructure specialists in main branch.

## Worktree Structure
```
project-root/               # Main branch (orchestrator)
├── .git/
└── worktrees/
    ├── backend-work/       # Backend specialist worktree
    ├── frontend-work/      # Frontend specialist worktree  
    ├── qa-work/           # QA specialist worktree
    └── docs-work/         # Docs specialist worktree

# DevOps & Security work directly in main branch
# (Project-wide infrastructure & security changes)
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

## Specialist Workflow Patterns

### Development Specialists (Use Worktrees)
- **Backend, Frontend, QA, Docs**: Dedicated worktrees for parallel development
- **Pattern**: Iterative development, session recovery, conflict isolation
- **Branches**: `backend-work`, `frontend-work`, `qa-work`, `docs-work`

### Infrastructure Specialists (Use Main Branch)
- **DevOps, Security**: Work directly in main branch with careful coordination
- **Pattern**: Project-wide analysis, infrastructure changes, compliance reviews
- **Workflow**: 
  ```bash
  # DevOps/Security workflow
  cd project-root  # Work in main branch
  git pull origin main  # Always start with latest
  # Make infrastructure/security changes
  git commit -m "DevOps task-{id}: {description} (confidence: {level})"
  ```

### Branch Naming Convention
- `backend-work` - Backend specialist development worktree
- `frontend-work` - Frontend specialist development worktree
- `qa-work` - QA specialist development worktree  
- `docs-work` - Documentation specialist worktree
- `main` - DevOps and Security work here directly
- `feature/{task-id}` - Optional for complex multi-specialist features

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

## Orchestration Timing Patterns

### Parallel Development Triggers
```
Feature Development Phase:
├── Backend task-001 (parallel) → worktrees/backend-work
├── Frontend task-002 (parallel) → worktrees/frontend-work  
├── QA task-003 (parallel) → worktrees/qa-work
└── Docs task-004 (parallel) → worktrees/docs-work
```

### Sequential Coordination Triggers
```
Infrastructure Phase:
├── DevOps task-005 (sequential) → main branch
└── After infrastructure: Resume parallel development

Security Review Phase:
├── Security task-006 (sequential) → main branch  
└── After security: Resume parallel development
```

### Optimal Orchestration Flow
1. **Start**: Parallel development specialists work on core features
2. **Infrastructure Need**: Sequential DevOps intervention when required
3. **Security Review**: Sequential Security analysis at milestones
4. **Resume**: Return to parallel development pattern

## Commands Integration
- `/git-status` - Show all worktree statuses and active tasks
- `/git-sync` - Update all worktrees from main branch
- `/git-merge task-{id}` - Merge specific completed task
- `/coordinate devops` - Sequential DevOps intervention
- `/coordinate security` - Sequential Security review