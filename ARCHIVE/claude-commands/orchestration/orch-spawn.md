# Specialist Spawning Command
# Usage: /orch-spawn <task-id> <specialist-type>

Create and initialize specialist context for task execution:

## Spawning Logic

### Step 1: Task Loading
- Load task JSON from appropriate folder (todo/, in_progress/, etc.)
- Validate task is ready for specialist assignment
- Extract specialist type and required context

### Step 2: Specialist Context Creation
**For new tasks (todo → in_progress):**
- Update task status: `todo` → `in_progress`
- Move task file: `/todo/task-X.json` → `/in_progress/task-X.json`
- Add log entry: "Started by [specialist] at [timestamp]"

**For resuming tasks (already in_progress):**
- Load existing task with full log history
- Instruct the specialist on additional relevant information from your higher project perspective, such as another specialist will be working in a similar area.
- Add log entry: "Resumed by [specialist] at [timestamp]"

### Step 3: Specialist Initialization
**Context Package:**
Initiate {specialist} using format:
`/.orchestrator/templates/context-package-template.json`



## Anti-Circular Design
- **One-way spawning**: Orchestrator → Specialist (no callback)
- **Independent operation**: Specialist works autonomously
- **State updates**: Specialist updates task JSON directly
- **Completion signal**: Specialist moves task to `/review/` or `/completed/`

## Parallel Safety
- **Different specialists**: Can work simultaneously on different tasks
- **Same specialist**: Sequential task assignment (finish current first)
- **Folder isolation**: Each state has separate directory
- **Git isolation**: Each specialist has separate worktree

**Result**: Clean specialist spawning without circular dependencies.