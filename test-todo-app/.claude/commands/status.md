# Task Status Analysis

Read all JSON files in .cc-orchestrator/tasks/ and output a status summary.

## Output Format
```json
{
  "summary": {
    "total": 0,
    "todo": 0,
    "in_progress": 0,
    "review": 0,
    "blocked": 0,
    "completed": 0
  },
  "tasks": {
    "todo": [],
    "in_progress": [],
    "review": [],
    "blocked": [],
    "completed": []
  },
  "can_assign_new": true,
  "next_action": "string"
}
```

## Analysis Rules
1. Count tasks by status field
2. Group tasks by status with id, title, type
3. can_assign_new = true only if in_progress count is 0
4. next_action based on priority:
   - "review_work" if review > 0
   - "monitor_progress" if in_progress > 0  
   - "assign_todo" if todo > 0 and can_assign_new
   - "resolve_blocked" if blocked > 0
   - "all_complete" if only completed tasks

## Task Structure Expected
```json
{
  "id": number,
  "title": "string",
  "type": "backend|frontend|qa|docs",
  "status": "todo|in_progress|review|blocked|completed",
  "confidence_assessment": {} // optional, present when specialist completes
}
```