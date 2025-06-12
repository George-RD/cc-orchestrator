# /orchestrate - Smart Orchestration Command

## Purpose
Intelligent state detection and action command that adapts based on current project state.

## Behavior Logic - Auto Command Delegation
```
if PRD exists && no active tasks:
  → Call /orchestration/tasks-create to parse PRD and create task breakdown
  
if active tasks exist:
  → Call /orchestration/status to show current state
  → Call /orchestration/orchestrate-resume for resumption options
  
if no PRD && no tasks:
  → Call /orchestration/prd-init to guide PRD creation using template
```

## Usage Examples
```bash
/orchestrate                    # Auto-detect and delegate to appropriate command
/orchestrate create            # Force call /orchestration/tasks-create
/orchestrate resume            # Force call /orchestration/orchestrate-resume
/orchestrate status            # Force call /orchestration/status
/orchestrate init              # Force call /orchestration/prd-init
```

## Auto-Delegation Map
**State Detection → Command Routing:**
- **No PRD, No Tasks** → `/orchestration/prd-init`
- **PRD exists, No Tasks** → `/orchestration/tasks-create` 
- **Active Tasks exist** → `/orchestration/status` then `/orchestration/orchestrate-resume`
- **Force modes** → Direct command calls

## Implementation
1. **Minimal Context Load**: Only check directory existence, not file contents
2. **Smart Routing**: Delegate heavy lifting to specialized commands
3. **State Detection**: 
   - Check `/.orchestrator/requirements/active/` for PRDs (file count only)
   - Check `/.orchestrator/tasks/active/` for tasks (file count only)
   - Route to appropriate command based on state
4. **Full Auto**: Enable completely autonomous operation through command chaining