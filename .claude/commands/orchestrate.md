# /orchestrate - Smart Orchestration Command

## Purpose
Intelligent state detection and action command that adapts based on current project state.

## Behavior Logic - Auto Command Delegation

```mermaid
flowchart TD
    A["/orchestrate called"] --> B{Check in_progress/ folder}
    B -->|Has tasks| C["🚨 PRIORITY: Call /orch-resume<br/>Continue active work"]
    B -->|Empty| D{Check todo/ folder}
    D -->|Has tasks| E["📋 Call /orch-status + /orch-resume<br/>Show available work + assign"]
    D -->|Empty| F{Check requirements/active/ for PRDs}
    F -->|Has PRDs| G["📝 Call /orch-tasks-create<br/>Parse PRD → create tasks"]
    F -->|No PRDs| H["🔧 Call /orch-prd-init<br/>Guide PRD creation"]
    
    C --> I[["🎯 Load specialist with full context<br/>Continue where left off"]]
    E --> J[["📋 Show available work<br/>Assign new tasks to specialists"]]
    G --> K[["🔨 Generate todo tasks<br/>Ready for assignment"]]
    H --> L[["📄 Create new PRD<br/>Start project planning"]]
    
    style C fill:#ff6b6b,color:#fff
    style A fill:#4ecdc4,color:#fff
    style I fill:#ff6b6b,color:#fff
```

**Priority Order**: in_progress → todo → PRD creation → initialization

## Task State Flow

```mermaid
stateDiagram-v2
    [*] --> todo : Task created
    todo --> in_progress : Specialist starts work
    in_progress --> review : Work completed
    in_progress --> blocked : Issue encountered
    blocked --> in_progress : Issue resolved
    review --> completed : Validation passed
    review --> in_progress : Changes requested
    completed --> [*]
    
    todo --> rejected : Task rejected
    in_progress --> rejected : Task abandoned
    rejected --> [*]
```

## Auto-Delegation Map
**State Detection → Command Routing (Priority Order):**
1. **In-Progress Tasks exist** → `/orch-resume` (IMMEDIATE)
2. **Todo Tasks exist (no in-progress)** → `/orch-status` then `/orch-resume`
3. **PRD exists, No Tasks** → `/orch-tasks-create` 
4. **No PRD, No Tasks** → `/orch-prd-init`
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