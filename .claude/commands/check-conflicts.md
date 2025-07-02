# File Conflict Detection

Analyze tasks for file conflicts to determine which can be done in parallel.

## Input
Object containing todo and in_progress tasks:
```json
{
  "todo": [
    {"id": 1, "files": ["src/api/users.js", "src/models/user.js"]},
    {"id": 2, "files": ["src/components/Login.jsx"]}
  ],
  "in_progress": [
    {"id": 3, "files": ["src/api/auth.js", "src/models/user.js"]}
  ]
}
```

## Output
```json
{
  "conflicts": {
    "1": [3],    // Task 1 conflicts with task 3 (both use src/models/user.js)
    "2": []      // Task 2 conflicts with no other tasks
  },
  "safe_groups": [
    [2]          // Only task 2 can be done without conflicts
  ],
  "conflicts_with_current": [1],  // Tasks that conflict with in_progress work
  "max_parallel": 1
}
```

## Conflict Rules
- Two tasks conflict if they share any files
- Tasks in same safe_group have no shared files among themselves
- Check todo tasks against in_progress tasks for conflicts
- max_parallel = size of largest safe group that doesn't conflict with current work

## Algorithm
1. Build file-to-task mapping for all tasks (todo + in_progress)
2. Find tasks that share files
3. Group non-conflicting todo tasks
4. Identify which todo tasks conflict with in_progress work
5. Return structured analysis with conflict information