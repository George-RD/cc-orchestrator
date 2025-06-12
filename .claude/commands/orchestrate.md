# /orchestrate - Smart Orchestration Command

## Purpose
Intelligent state detection and action command that adapts based on current project state.

## Behavior Logic - Auto Command Delegation
```
if PRD exists && no active tasks:
  → Call /tasks-create to parse PRD and create task breakdown
  
if active tasks exist:
  → Call /status to show current state
  → Call /orchestrate-resume for resumption options
  
if no PRD && no tasks:
  → Call /prd-init to guide PRD creation using template
  
if tasks blocked or need update:
  → Call /task-update for progress management
```

## Usage Examples
```bash
/orchestrate                    # Auto-detect and delegate to appropriate command
/orchestrate create            # Force call /tasks-create
/orchestrate resume            # Force call /orchestrate-resume
/orchestrate status            # Force call /status
/orchestrate init              # Force call /prd-init
```

## Auto-Delegation Map
**State Detection → Command Routing:**
- **No PRD, No Tasks** → `/prd-init`
- **PRD exists, No Tasks** → `/tasks-create` 
- **Active Tasks exist** → `/status` then `/orchestrate-resume`
- **Tasks blocked** → `/task-update`
- **Force modes** → Direct command calls

## Implementation
1. **Minimal Context Load**: Only check directory existence, not file contents
2. **Smart Routing**: Delegate heavy lifting to specialized commands
3. **State Detection**: 
   - Check `/.orchestrator/requirements/active/` for PRDs (file count only)
   - Check `/.orchestrator/tasks/active/` for tasks (file count only)
   - Route to appropriate command based on state
4. **Full Auto**: Enable completely autonomous operation through command chaining