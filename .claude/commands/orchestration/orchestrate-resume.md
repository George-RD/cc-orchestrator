# Resume Orchestration
# Usage: /orchestrate-resume

Simple logic to resume work on existing tasks:

## Resume Logic

### Step 1: Show Current State
**Display active tasks:**
- List all files in `/.orchestrator/tasks/active/`
- Show task titles and types
- Highlight high priority tasks

### Step 2: Task State Analysis
**Task states and actions:**
- **active**: Ready to start - can transition to in_progress
- **in_progress**: Currently being worked on - add log entries as you work  
- **blocked**: Cannot proceed - identify and resolve blockers
- **review**: Completed work - needs validation before completion
- **completed**: Fully done - move to completed directory

### Step 3: Resumption Recommendations
**Smart suggestions based on states:**
```
If active tasks (different specialists):
  → "Start parallel work: Task X (docs), Task Y (qa), Task Z (backend)"

If in_progress tasks exist:
  → "Continue working on Task X - add log entries as you progress"

If blocked tasks exist:
  → "Resolve blockers for Task X, then transition to in_progress"

If review tasks exist:
  → "Validate completed work in Task X, then mark completed"
```

### Step 4: Git Worktree Guidance
**For parallel work:**
- Suggest creating `worktrees/{specialist}-work` branches
- Simple git workflow instructions
- Basic conflict resolution guidance

## Simplicity Rules
1. **No complex state reconstruction** - Just show current files
2. **Basic recommendations** - Parallel vs sequential logic  
3. **Clear next steps** - What to do, not how to do it
4. **Git optional** - Works with or without worktrees

**Result**: Clear resumption path based on current task state.