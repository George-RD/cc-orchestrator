# Update Task Progress
# Usage: /task-update <task-id> <progress|decision|blocker|complete>

Update task state and maintain work history:

## 1. Load Task

```bash
task_file="/.orchestrator/tasks/active/${1}.json"
if [ ! -f "$task_file" ]; then
    echo "❌ Task not found: ${1}"
    exit 1
fi
```

## 2. Update Types

### 2.1 Progress Update
`/task-update task-001 progress`

Append to work_log:
```json
{
  "timestamp": "2025-01-15T10:30:00Z",
  "agent": "backend",
  "action": "Implemented user authentication endpoints",
  "details": "Created login, logout, and refresh token endpoints with tests",
  "confidence": 0.92,
  "files_created": [
    "src/api/auth/login.js",
    "src/api/auth/logout.js",
    "tests/api/auth.test.js"
  ],
  "files_modified": [
    "src/routes/auth.js",
    "src/middleware/auth.js"
  ],
  "progress_percentage": 75
}
```

### 2.2 Decision Documentation
`/task-update task-001 decision`

Record architectural/technical decisions:
```json
{
  "timestamp": "2025-01-15T11:00:00Z",
  "agent": "backend",
  "action": "Technical decision: JWT vs Session auth",
  "details": "Chose JWT for stateless architecture and mobile app support",
  "decisions": [{
    "type": "technical",
    "title": "Authentication method",
    "choice": "JWT tokens",
    "alternatives": ["Session cookies", "OAuth only"],
    "rationale": "Better for distributed system and mobile clients"
  }],
  "impact": "Will need token refresh mechanism"
}
```

### 2.3 Blocker Encountered
`/task-update task-001 blocker`

Document what's preventing progress:
```json
{
  "timestamp": "2025-01-15T14:00:00Z",
  "agent": "frontend",
  "action": "Blocked: Waiting for API endpoints",
  "blockers": [{
    "type": "dependency",
    "blocking_task": "task-backend-001",
    "description": "Need auth endpoints to test login form",
    "estimated_wait": "2-3 hours"
  }],
  "status": "blocked"
}
```

Move task to blocked directory:
```bash
mv /.orchestrator/tasks/active/${1}.json /.orchestrator/tasks/blocked/${1}.json
```

### 2.4 Task Completion
`/task-update task-001 complete`

Finalize task:
```json
{
  "timestamp": "2025-01-15T16:00:00Z",
  "agent": "backend",
  "action": "Task completed",
  "details": "All acceptance criteria met, tests passing",
  "deliverables": [
    "Authentication API endpoints",
    "Unit and integration tests",
    "API documentation"
  ],
  "completion": {
    "completed_at": "2025-01-15T16:00:00Z",
    "completed_by": "backend",
    "test_results": "15/15 passing",
    "coverage": "92%"
  },
  "progress_percentage": 100
}
```

## 3. State Transitions

### Update task status based on action:
```yaml
status_transitions:
  progress:
    if: progress < 100
    then: status = "in_progress"
    
  decision:
    # Status unchanged, just document
    
  blocker:
    then: status = "blocked"
    move_to: /.orchestrator/tasks/blocked/
    
  complete:
    if: all_criteria_met
    then: status = "review"
    else: status = "in_progress" with note
```

## 4. Update Registry

Sync registry.json with changes:
```json
{
  "summary": {
    "by_status": {
      "in_progress": -1,
      "review": +1
    }
  },
  "active_tasks": {
    "task-001": {
      "status": "review",
      "progress": 100,
      "last_update": "2025-01-15T16:00:00Z"
    }
  }
}
```

## 5. Trigger Side Effects

Based on update type:

### On Progress:
- Check if this unblocks other tasks
- Update parent epic progress if exists

### On Blocker:
- Notify orchestrator 
- Add to blocked task list
- Suggest alternative work

### On Complete:
- Trigger review process
- Check dependent tasks
- Update PRD progress

## 6. Output Confirmation

```
✅ Task Updated Successfully
════════════════════════════════════════════

Task: ${task_id}
Update Type: ${update_type}
New Status: ${new_status}
Progress: ${progress}%

${contextual_message}

Next Actions:
- ${suggested_next_action}
```

## 7. Work Log Best Practices

Each work_log entry should:
- Be atomic (one logical change)
- Include concrete details
- List affected files
- Document decisions
- Update progress accurately

## 8. Error Handling

```yaml
validation:
  - Task must exist
  - Agent must be assigned to task
  - Progress must be 0-100
  - Status transitions must be valid
  - Required fields must be present
```

---
*Task updated. Work history preserved for resumption.*
