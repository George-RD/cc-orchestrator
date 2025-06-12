# Resume Orchestration  
# Usage: /orchestrate-resume

Resume work on existing tasks by reconstructing context from persistent state:

## 1. Scan Current State

```bash
# Find all active tasks
active_tasks=$(ls /.orchestrator/tasks/active/*.json 2>/dev/null)
blocked_tasks=$(ls /.orchestrator/tasks/blocked/*.json 2>/dev/null)

# Check for uncommitted work
uncommitted=$(git status --porcelain)

# Load registry summary for overview
summary=$(jq '.summary' /.orchestrator/tasks/registry.json)
```

## 2. Reconstruct Context

For each active task, gather:

### 2.1 Task State
```json
# From task JSON file:
- Current status and progress
- Assigned agent
- Last work_log entry
- Pending deliverables
```

### 2.2 Work Context
```bash
# Check for work in progress:
- Uncommitted files related to task
- Test results (passing/failing)
- Open pull requests
- Partial implementations
```

### 2.3 Dependencies
```yaml
# Analyze blocking relationships:
- Tasks this blocks
- Tasks blocking this
- Completion status of dependencies
```

## 3. Priority Assessment

Determine work order based on:

```yaml
priority_calculation:
  factors:
    - task_priority: critical=4, high=3, normal=2, low=1
    - blocking_others: +2 points per blocked task
    - close_to_completion: +1 if >75% complete
    - has_deadline: +3 if deadline within 2 days
    
  sort_by: total_points DESC
```

## 4. Agent Re-engagement

For the highest priority task:

### 4.1 If task was in progress:
```markdown
📋 Resuming Task: ${task_id}
════════════════════════════════════════════

Previous Progress:
- Status: ${status} (${progress}% complete)
- Last worked: ${last_timestamp}
- By agent: ${agent}

Last Activity:
"${last_work_log_entry}"

Artifacts Created:
- ${file_1}
- ${file_2}

Next Steps from Task:
1. ${next_step_1}
2. ${next_step_2}

Uncommitted Changes Detected:
- ${changed_file_1}
- ${changed_file_2}

Recommended Action:
[ Continue with ${agent} on ${next_action} ]
```

### 4.2 If task was blocked:
```markdown
🚧 Blocked Task: ${task_id}
════════════════════════════════════════════

Blocked by: ${blocking_task}
- Status: ${blocking_status}
- Progress: ${blocking_progress}%

Recommended Action:
[ Focus on completing ${blocking_task} first ]
```

## 5. Multi-Task Coordination

If multiple tasks can proceed in parallel:

```markdown
🔄 Parallel Work Available
════════════════════════════════════════════

Can proceed simultaneously:
1. task-backend-001 → Backend API [backend agent]
2. task-frontend-001 → UI Components [frontend agent]
3. task-docs-001 → API Documentation [docs agent]

Orchestration Strategy:
- Assign each to respective specialist
- Set sync point when all complete
- Monitor for integration needs
```

## 6. Context Handoff

Prepare focused context for each agent:

```yaml
handoff_package:
  task_id: "task-xxx-001"
  agent: "backend"
  
  essential_context:
    current_state: "Implementation 50% complete"
    completed: ["Database schema", "Basic endpoints"]
    remaining: ["Authentication", "Error handling"]
    
  technical_context:
    files_to_review: ["src/api/users.js"]
    test_status: "3 passing, 2 failing"
    
  immediate_action: "Fix failing auth tests"
```

## 7. Progress Tracking

Update registry after resume:

```json
{
  "work_sessions": {
    "session_resumed": "2025-01-15T10:00:00Z",
    "tasks_activated": ["task-001", "task-002"],
    "agents_engaged": ["backend", "frontend"]
  }
}
```

## 8. Resume Summary Output

```
🔄 Orchestration Resumed
════════════════════════════════════════════

Active Work:
1. ⚡ task-auth-001: Backend API [75% complete]
   → Continue implementation with backend agent
   
2. 🎨 task-ui-001: Login Form [30% complete]  
   → Waiting on API completion
   
3. 📝 task-docs-001: API Docs [0% complete]
   → Can start in parallel

Blocked Tasks: 2
- task-test-001: Needs API completion
- task-deploy-001: Needs all tests passing

Recommended Priority:
→ Complete task-auth-001 first (blocks 3 others)

Ready to continue? 
- Proceed with recommendation: /task-continue task-auth-001
- View different task: /task-detail task-xxx-001
- Change priority: /task-prioritize
```

## 9. Error Recovery

If state seems corrupted or incomplete:

```yaml
recovery_protocol:
  - Check git history for last known good state
  - Review task work_logs for context  
  - Scan filesystem for orphaned work
  - Reconstruct from available artifacts
  - Mark questionable tasks for review
```

---
*Work resumed from persistent state. Continue with highest priority task.*
