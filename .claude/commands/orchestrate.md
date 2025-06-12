# /orchestrate - Smart Orchestration Command

## Purpose
Intelligent state detection and action command that adapts based on current project state.

## Behavior Logic - Auto Command Delegation
```
if in_progress tasks exist:
  → PRIORITY: Call /project/orchestration/orchestrate-resume to continue active work
  
if todo tasks exist (no in_progress):
  → Call /project/orchestration/status to show available work
  → Call /project/orchestration/orchestrate-resume for task assignment
  
if PRD exists && no tasks:
  → Call /project/orchestration/tasks-create to parse PRD and create task breakdown
  
if no PRD && no tasks:
  → Call /orchestration/prd-init to guide PRD creation using template
```

## Auto-Delegation Map
**State Detection → Command Routing (Priority Order):**
1. **In-Progress Tasks exist** → `/project/orchestration/orchestrate-resume` (IMMEDIATE)
2. **Todo Tasks exist (no in-progress)** → `/project/orchestration/status` then `/project/orchestration/orchestrate-resume`
3. **PRD exists, No Tasks** → `/project/orchestration/tasks-create` 
4. **No PRD, No Tasks** → `/project/orchestration/prd-init`
5. **Force modes** → Direct command calls

## Implementation
1. **Minimal Context Load**: Only check directory existence, not file contents
2. **Smart Routing**: Delegate heavy lifting to specialized commands
3. **State Detection (Priority Order)**: 
   - **FIRST**: Check `/.orchestrator/tasks/in_progress/` for active work (immediate priority)
   - **SECOND**: Check `/.orchestrator/tasks/todo/` for available work
   - **THIRD**: Check `/.orchestrator/requirements/active/` for PRDs to create tasks
   - Route to appropriate command based on highest priority state found
4. **Full Auto**: Enable completely autonomous operation through command chaining