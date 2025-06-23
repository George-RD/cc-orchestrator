# /orchestrate

Execute the main orchestration loop following the logic below.

## Core Logic

```python
while True:
    # Get current status via headless command
    status = get_status_headless()  # cat .claude/commands/status.md | claude -p --output-format json
    
    # PRIORITY ORDER (stop at first match):
    
    # 1. Review tasks have highest priority
    if status.review_tasks:
        for task_id in status.review_tasks:
            spawn_tdd_reviewer(task_id)
            # After reviewer completes, check if passed/failed
            
    # 2. Check in-progress tasks for stalls
    elif status.in_progress_tasks:
        for task_id in status.in_progress_tasks:
            task = read_task(task_id)
            # Task is stalled if it has confidence_assessment but still in_progress
            if task.get("confidence_assessment"):
                update_task_status(task_id, "review")
            # Or if it seems abandoned (you just checked after Task() returned)
            elif task_was_just_assigned and still_in_progress:
                reset_task_to_todo(task_id, "Specialist failed to start work")
                
    # 3. Assign new work
    elif status.todo_tasks:
        # Get all todo tasks with their dependencies
        available_tasks = [t for t in status.todo_tasks if dependencies_met(t)]
        
        if available_tasks:
            # Check which tasks can run together without file conflicts
            conflict_analysis = check_conflicts_headless(available_tasks)
            # Returns: {"safe_groups": [[1,2], [3,4,5]], "max_parallel": 3}
            
            # Select a group of non-conflicting tasks
            for group in conflict_analysis.safe_groups:
                # Check if any tasks in this group are already in progress
                group_available = all(
                    task_id not in status.in_progress_ids 
                    for task_id in group
                )
                
                if group_available:
                    # Assign tasks in this group
                    for task_id in group:
                        task = get_task(task_id)
                        # Spawn specialist with common instructions
                        result = Task(
                            description=f"{task.type} specialist - Task {task.id}",
                            prompt=f"""You are a {task.type} specialist.

COMMON INSTRUCTIONS FOR ALL SPECIALISTS:
1. Read your role definition: /.cc-orchestrator/specialists/{task.type}.md
2. Read your task: /.cc-orchestrator/tasks/task-{task.id}.json
3. For efficient searching, use rg (ripgrep) instead of grep
4. Follow TDD:
   - Write FAILING tests first
   - Run tests to ensure they fail
   - Implement until tests pass
   - Do NOT modify tests to make them pass
5. Code quality standards:
   - Clear, self-documenting names
   - Single responsibility per function/class
   - Proper error handling
   - No code without tests
6. Repository organization:
   - Keep files in appropriate directories (src/, tests/, docs/, etc.)
   - Don't clutter the root directory
   - Update README/docs when adding major features
7. When complete, update the task JSON with:
   {{"status": "review", "confidence_assessment": {{"level": "high|medium|low", "notes": "explanation"}}}}
   - High: All criteria met, tests pass, follows patterns
   - Medium: Good solution, minor concerns noted
   - Low: Blockers exist, needs help
8. If blocked, update: {{"status": "blocked", "blocker_reason": "explanation"}}
9. Commit your changes with clear messages

CRITICAL: You MUST update the task JSON status before finishing!
Start work immediately."""
                        )
                        
                        # IMMEDIATELY check what happened
                        task_after = read_task(task.id)
                        if task_after.status == "todo":
                            # Specialist didn't even start
                            add_to_task_log(task.id, "Specialist failed to update status")
                    
                    # Break after assigning first available group
                    break
                
    # 4. Handle blocked tasks
    elif status.blocked_tasks:
        for task_id in status.blocked_tasks:
            task = read_task(task_id)
            # Add guidance and reset to todo
            update_task(task_id, {
                "status": "todo",
                "previous_blocker": task.blocker_reason,
                "guidance": "Try a different approach or break down the task"
            })
            
    # 5. All done!
    else:
        if all(t.status == "completed" for t in all_tasks):
            print("✅ All tasks completed!")
            break
        else:
            # Wait a bit before checking again
            sleep(30)

def spawn_tdd_reviewer(task_id):
    """Spawn a TDD reviewer for a task in review status"""
    result = Task(
        description=f"TDD Review for task {task_id}",
        prompt=f"""You are a TDD review specialist.

Review task: /.cc-orchestrator/tasks/task-{task_id}.json

1. Check git history for test-first development
2. Verify tests were written before implementation  
3. Ensure tests weren't modified to pass
4. Return your verdict by updating the task with:
   - If passed: {{"status": "completed", "review_passed": true}}
   - If failed: {{"status": "todo", "review_feedback": "what went wrong"}}"""
    )
    
def get_status_headless():
    """Get task status summary without loading task details"""
    # cat .claude/commands/status.md | claude -p --output-format json
    
def check_conflicts_headless(todo_tasks):
    """Check which tasks can run in parallel"""
    # echo "$TASKS" | claude -p < .claude/commands/check-conflicts.md
```

## Key Principles

1. **Non-conflicting work** - Assign tasks that don't have file conflicts
2. **Immediate reaction** - Check task status right after each Task() returns
3. **Stall detection** - Reset tasks that get stuck
4. **Clear priorities** - Review → Monitor → Assign → Blocked
5. **Common instructions** - All specialists get the same base requirements