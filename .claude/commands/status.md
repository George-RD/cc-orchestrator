# Task Status Analysis

Analyze task status by combining JSON files and git state, then reconcile any discrepancies.

## Process Flow
1. Read all JSON files in .cc-orchestrator/tasks/ (parallel with step 2)
2. Run git-sync-check to analyze branch state (parallel with step 1):
   `cat .claude/commands/git-sync-check.md | claude -p --output-format json`
3. Reconcile task status with git state
4. Output enhanced status summary

## Output Format
```json
{
  "summary": {
    "total": 0,
    "available_todo": 0,
    "dependent_tasks": 0,
    "in_progress": 0,
    "review": 0,
    "needs_assistance": 0,
    "completed": 0
  },
  "tasks": {
    "available_todo": [],
    "dependent_tasks": [],
    "in_progress": [],
    "review": [],
    "needs_assistance": [],
    "completed": []
  },
  "can_assign_new": true,
  "next_action": "string",
  "git_sync_issues": []
}
```

## Analysis Rules
1. Count tasks by status field
2. Group tasks by status with id, title, type, priority, git_state
3. For todo tasks, evaluate dependencies:
   - If all dependencies have status "completed" → available_todo (sorted by priority: high → medium → low)
   - If any dependency is not completed → dependent_tasks
4. can_assign_new = true only if in_progress count is 0
5. For in_progress tasks, check if they have confidence_assessment (indicates completion)
6. Reconcile git state with task status:
   - Task "in_progress" but no branch → Add sync issue, suggest "todo"
   - Task "in_progress" with branch but no recent commits → Add stall warning
   - Task "review" but branch shows active commits → Add sync issue
   - Task "completed" but branch not merged → Add sync issue
   - Orphaned branches → Add cleanup suggestion
7. next_action based on priority:
   - "fix_sync_issues" if git_sync_issues > 0
   - "review_work" if review > 0
   - "check_stalled" if in_progress > 0 with confidence_assessment or stale branch
   - "wait_for_progress" if in_progress > 0 without issues
   - "assign_todo" if available_todo > 0 and can_assign_new
   - "resolve_assistance" if needs_assistance > 0
   - "all_complete" if all tasks have status completed

## Task Structure Expected
```json
{
  "id": number,
  "title": "string",
  "type": "backend|frontend|qa|docs",
  "status": "todo|in_progress|review|needs_assistance|completed",
  "dependencies": [],
  "priority": "high|medium|low",
  "confidence_assessment": {} // optional, present when specialist completes
}
```

## Enhanced Task Output Format
Tasks in output will include git state:
```json
{
  "id": 1,
  "title": "Create user API",
  "type": "backend",
  "priority": "high",
  "git_state": {
    "branch_exists": true,
    "commits_ahead": 5,
    "last_activity": "2024-01-01T12:00:00Z",
    "tdd_compliance": true
  }
}
```

## Git Sync Issues Format
```json
{
  "type": "status_mismatch|stale_branch|orphaned_branch",
  "task_id": 1,
  "description": "Task marked in_progress but no feature branch exists",
  "suggestion": "Reset task to todo or create branch task-1-backend"
}
```