# /orchestrate - Smart Orchestration Command

## Purpose
Intelligent state detection and action command that adapts based on current project state.

## Behavior Logic
```
if PRD exists && no active tasks:
  → Parse PRD and create task breakdown
  
if active tasks exist:
  → Show status and resume work
  
if no PRD && no tasks:
  → Guide user to create PRD using template
```

## Usage Examples
```bash
/orchestrate                    # Auto-detect and act
/orchestrate create            # Force create tasks from PRD
/orchestrate resume            # Force resume existing work  
/orchestrate status            # Show current state only
```

## Implementation
The command checks:
1. `/.orchestrator/requirements/active/` for PRDs
2. `/.orchestrator/tasks/active/` for ongoing work
3. Provides appropriate next steps based on findings