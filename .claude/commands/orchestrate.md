# /orchestrate

Execute the main orchestration loop following the logic below.

## Core Logic

```python
while True:
    # Get current status via headless command
    status = get_status_headless()  # cat .claude/commands/status.md | claude -p --output-format json
    
    # Handle all priorities in same cycle:
    
    # 1. Review tasks have highest priority - process ALL reviews
    if status.review_tasks:
        for task_id in status.review_tasks:
            task = read_task(task_id)
            spawn_tdd_reviewer(task_id, task.type)
            # After reviewer completes, check if passed/failed
            
    # 2. Check in-progress tasks for stalls (run regardless of reviews)
    if status.in_progress_tasks:
        for task_id in status.in_progress_tasks:
            task = read_task(task_id)
            # Task is stalled if it has confidence_assessment but still in_progress
            if task.get("confidence_assessment"):
                update_task_status(task_id, "review")
            # Or if it seems abandoned (you just checked after Task() returned)
            elif task_was_just_assigned and still_in_progress:
                reset_task_to_todo(task_id, "Specialist failed to start work")
                
    # 3. Assign new work based on dependencies and conflicts
    if status.todo_tasks:
        # Get all todo tasks with their dependencies
        # For now, assume all dependencies are met (can be enhanced later)
        available_tasks = status.todo_tasks
        
        if available_tasks:
            # Check which tasks can run together without file conflicts
            # considering what's already in progress
            current_in_progress = status.in_progress_tasks
            conflict_analysis = check_conflicts_headless(available_tasks, current_in_progress)
            # Returns: {"safe_groups": [[1,2], [3,4,5]], "conflicts_with_current": [6,7]}
            
            # Assign tasks that don't conflict with current work
            for group in conflict_analysis.safe_groups:
                # Filter out tasks that would conflict with current work
                safe_to_start = [
                    task_id for task_id in group 
                    if task_id not in conflict_analysis.get('conflicts_with_current', [])
                ]
                
                if safe_to_start:
                    # Assign all safe tasks
                    for task_id in safe_to_start:
                        task = get_task(task_id)
                        # Spawn specialist with common instructions
                        result = Task(
                            description=f"{task.type} specialist - Task {task.id}",
                            prompt=f"""You are a {task.type} specialist.

COMMON INSTRUCTIONS FOR ALL SPECIALISTS:
1. Read your role definition: /.cc-orchestrator/specialists/{task.type}.md
2. Read your task: /.cc-orchestrator/tasks/task-{task.id}.json
3. IMMEDIATELY update task status to "in_progress": {{"status": "in_progress"}}
4. Check for existing feature branch and switch/create:
   - First check: git branch -l task-{task.id}-{task.type}
   - If exists: git checkout task-{task.id}-{task.type}
     Then review commit history: git log --oneline -10
     Understand what's been done and continue from there
   - If not exists: git checkout -b task-{task.id}-{task.type}
5. For efficient searching, use rg (ripgrep) instead of grep
6. STRICT TDD WORKFLOW (CRITICAL - You will be reviewed on this):
   For EACH feature/function:
   a) Write ONE failing test
   b) Run test to verify it fails
   c) Commit: "Add failing test for [feature]"
   d) Write MINIMAL code to make test pass
   e) Run test to verify it passes
   f) Commit: "Implement [feature] to pass test"
   g) Refactor if needed
   h) Commit: "Refactor [feature]" (if applicable)
   i) Repeat for next feature
   
   NEVER:
   - Write multiple tests at once
   - Commit tests and implementation together
   - Delete or replace existing tests
   - Write implementation before test
7. Code quality standards:
   - Clear, self-documenting names
   - Single responsibility per function/class
   - Proper error handling
   - No code without tests
8. Repository organization:
   - Keep files in appropriate directories (src/, tests/, docs/, etc.)
   - Don't clutter the root directory
   - DO NOT create status reports or completion files
   - DO NOT create new documentation unless specified in task
   - Update existing README/docs ONLY when adding major features
9. Git workflow:
   - Work ONLY on your feature branch
   - Commit AFTER EVERY TDD STEP (test, implementation, refactor)
   - Each commit should be atomic and have a clear message
   - Example commit sequence:
     * "Add failing test for todo creation"
     * "Implement todo creation"
     * "Add failing test for todo toggle"
     * "Implement todo toggle functionality"
   - Do NOT merge to main - the TDD reviewer will merge if approved
10. When complete:
   - Update the task JSON with: {{"status": "review", "confidence_assessment": {{"level": "high|medium|low", "notes": "explanation"}}}}
   - High: All criteria met, tests pass, follows patterns
   - Medium: Good solution, minor concerns noted
   - Low: Blockers exist, needs help
   - DO NOT create completion reports or summary files
   - Task JSON update is your ONLY completion signal

CRITICAL: You MUST update the task JSON status before finishing!
If blocked at any point, update: {{"status": "blocked", "blocker_reason": "explanation"}}
Start work immediately.

IMPORTANT: You will be reviewed on:
- Test-first development (tests before implementation)
- Incremental commits showing red-green-refactor cycle
- Each commit should represent ONE small step
- Git history must clearly show TDD process"""
                        )
                        
                        # IMMEDIATELY check what happened
                        task_after = read_task(task.id)
                        if task_after.status == "todo":
                            # Specialist didn't even start - try once more
                            print(f"⚠️ Task {task.id} specialist failed to start, resetting...")
                            # Could spawn again or mark for manual intervention
                
    # 4. Handle blocked tasks
    if status.blocked_tasks:
        for task_id in status.blocked_tasks:
            task = read_task(task_id)
            # Add guidance and reset to todo
            update_task(task_id, {
                "status": "todo",
                "previous_blocker": task.blocker_reason,
                "guidance": "Try a different approach or break down the task"
            })
            
    # 5. Check if all done
    all_tasks = read_all_tasks()
    if all(t.status == "completed" for t in all_tasks):
        print("✅ All tasks completed!")
        break
    
    # Wait a bit before next cycle
    sleep(30)

def spawn_tdd_reviewer(task_id, task_type):
    """Spawn a TDD reviewer for a task in review status"""
    result = Task(
        description=f"TDD Review for task {task_id}",
        prompt=f"""You are a TDD review specialist.

Review task: /.cc-orchestrator/tasks/task-{task_id}.json
Task type: {task_type}

1. Switch to the feature branch: git checkout task-{task_id}-{task_type}
2. Check git history for test-first development: git log --oneline main..HEAD
3. Verify tests were written before implementation  
4. Ensure tests weren't modified to pass
5. If approved:
   - Merge to main: git checkout main && git merge task-{task_id}-{task_type}
   - Delete feature branch: git branch -d task-{task_id}-{task_type}
   - Update task: {{"status": "completed", "review_passed": true}}
6. If rejected:
   - Stay on feature branch
   - Update task: {{"status": "todo", "review_feedback": "what went wrong"}}"""
    )
    
def get_status_headless():
    """Get task status summary with git sync analysis"""
    # cat .claude/commands/status.md | claude -p --output-format json
    # This now runs both JSON analysis AND git-sync-check internally
    
def check_conflicts_headless(todo_tasks, in_progress_tasks):
    """Check which tasks can run in parallel considering current work"""
    # Combine both task lists with status markers
    # echo '{"todo": $TODO_TASKS, "in_progress": $IN_PROGRESS_TASKS}' | claude -p < .claude/commands/check-conflicts.md
```

## Key Principles

1. **Non-conflicting work** - Assign tasks that don't have file conflicts
2. **Immediate reaction** - Check task status right after each Task() returns
3. **Stall detection** - Reset tasks that get stuck
4. **Clear priorities** - Review → Monitor → Assign → Blocked
5. **Common instructions** - All specialists get the same base requirements

## Error Handling Examples

### Git Branch Issues
If specialist reports "fatal: A branch named 'task-X' already exists":
- This is expected for resumed tasks
- Specialist should checkout existing branch and continue

### Stalled Tasks
If task remains in_progress with no commits for >30min:
- Check git branch for activity
- Consider resetting to todo with guidance

### Failed Reviews
If TDD reviewer rejects work:
- Task returns to todo with feedback
- Next specialist continues from existing branch

### Sync Issues
If status.git_sync_issues detected:
- Priority becomes "fix_sync_issues"
- May need manual intervention for orphaned branches