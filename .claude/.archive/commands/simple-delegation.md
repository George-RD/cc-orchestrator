# Simple Delegation Logic

Basic rules for task coordination without complex algorithms:

## Delegation Rules

### Parallel Execution
**Tasks can run in parallel when:**
- Different specialist types (docs + qa + backend)
- No dependencies between tasks
- Different git worktrees

### Sequential Execution  
**Tasks must run sequentially when:**
- Same specialist type (all docs tasks)
- Dependencies exist (Task B waits for Task A)
- Shared resources (same files/directories)

### Agent Assignment
**Simple specialist mapping:**
- **docs**: File cleanup, documentation, templates
- **qa**: Testing, validation, quality checks
- **backend**: Logic, APIs, data processing
- **devops**: Infrastructure, deployment, CI/CD

## Decision Logic
```
For each task:
  if same_specialist_as_active_task:
    → Sequential (wait for completion)
  elif has_dependencies:
    → Sequential (wait for dependencies)  
  else:
    → Parallel (can start immediately)
```

**Result**: Simple, predictable coordination without complex algorithms.