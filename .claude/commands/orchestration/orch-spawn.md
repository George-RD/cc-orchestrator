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
- Preserve specialist context and progress
- Add log entry: "Resumed by [specialist] at [timestamp]"

### Step 3: Specialist Initialization
**Context Package:**
```
Task: {task JSON}
Specialist: {specialist type}
Context: {
  "role": "You are a [specialist] working on this specific task",
  "task_details": {task JSON},
  "last_log_entry": "Most recent progress",
  "acceptance_criteria": [list],
  "git_worktree": "worktrees/{specialist}-work"
}
Instructions: "Continue/Start work on this task. Update log as you progress."
```

### Step 4: Git Worktree Setup (Optional)
- Check if `worktrees/{specialist}-work` exists
- Create or update worktree as needed
- Switch specialist to appropriate branch

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