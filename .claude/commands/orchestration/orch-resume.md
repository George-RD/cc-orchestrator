# Resume Orchestration
# Usage: /orchestrate-resume

Simple logic to resume work on existing tasks:

## Resume Logic

### Step 1: Prioritize Current Work
**Check in_progress tasks FIRST:**
- List all files in `/.orchestrator/tasks/in_progress/` (PRIORITY)
- For each in_progress task: Load specialist, last log entry, current context
- These tasks must be continued before starting new work

**Then check todo tasks:**
- List all files in `/.orchestrator/tasks/todo/` (SECONDARY)
- Show task titles, types, and readiness to start
- Only suggest if no in_progress tasks exist

### Step 2: Task State Analysis
**Task states and actions:**
- **todo**: Ready to start - can transition to in_progress
- **in_progress**: Currently being worked on - add log entries as you work  
- **blocked**: Cannot proceed - identify and resolve blockers
- **review**: Completed work - needs validation before completion
- **completed**: Fully done - move to `/completed/` directory
- **rejected**: Task was rejected - moved to `/rejected/` directory

### Step 3: Resumption Recommendations (Priority Order)
**1. PRIORITY: In-Progress Tasks (MUST continue first)**
```
If in_progress tasks exist:
  → "RESUME: Continue Task X [specialist] - last worked on: [timestamp]"
  → "Context: [last log entry]"
  → "Next steps: [from acceptance criteria]"
  → Load specialist with full task context for seamless continuation
```

**2. SECONDARY: Todo Tasks (only if no in_progress)**
```
If no in_progress AND todo tasks exist:
  → "Available work: Task X (docs), Task Y (qa), Task Z (backend)"
  → "Recommend starting: [highest priority task]"
  → "Can run parallel: [tasks with different specialists]"
```

**3. Other States:**
```
If blocked tasks exist:
  → "Resolve blockers for Task X, then transition to in_progress"

If review tasks exist:
  → "Validate completed work in Task X, then mark completed"
```

### Step 4: Specialist Handoff Protocol
**For in_progress tasks (PRIORITY):**
- Load the specialist who was working on the task
- Provide: Task JSON, last log entry, acceptance criteria
- Context: "You were working on this. Continue where you left off."

**For todo tasks (SECONDARY):**
- Assign specialist based on task.type
- Provide: Fresh task JSON, full context
- Context: "New task assignment. Start from beginning."

**For git worktree coordination:**
- Check if specialist worktree exists
- Resume in existing worktree or create new one
- Maintain specialist isolation

## Priority Rules
1. **In-progress takes absolute priority** - Never interrupt active work
2. **Specialist continuity** - Same specialist resumes their work
3. **Context preservation** - Full task history loaded for resumption
4. **Seamless handoff** - Specialist picks up exactly where they left off

**Result**: Specialists can seamlessly continue interrupted work with full context.