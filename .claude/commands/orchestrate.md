# /orchestrate - Smart Orchestration Command

## Purpose
Intelligent state detection and action command that adapts based on current project state.

## Behavior Logic - Auto Command Delegation
```
if PRD exists && no todo tasks:
  → Call /project/orchestration/tasks-create to parse PRD and create task breakdown
  
if todo/in_progress tasks exist:
  → Call /project/orchestration/status to show current state
  → Call /project/orchestration/orchestrate-resume for resumption options
  
if no PRD && no tasks:
  → Call /orchestration/prd-init to guide PRD creation using template
```

## Auto-Delegation Map
**State Detection → Command Routing:**
- **No PRD, No Tasks** → `/project/orchestration/prd-init`
- **PRD exists, No Todo Tasks** → `/project/orchestration/tasks-create` 
- **Todo/In-Progress Tasks exist** → `/project/orchestration/status` then `/project/orchestration/orchestrate-resume`
- **Force modes** → Direct command calls

## Implementation
1. **Minimal Context Load**: Only check directory existence, not file contents
2. **Smart Routing**: Delegate heavy lifting to specialized commands
3. **State Detection**: 
   - Check `/.orchestrator/requirements/active/` for PRDs (file count only)
   - Check `/.orchestrator/tasks/todo/` and `/.orchestrator/tasks/in_progress/` for active work (file count only)
   - Route to appropriate command based on state
4. **Full Auto**: Enable completely autonomous operation through command chaining