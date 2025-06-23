# File Conflict Detection

Analyze tasks for file conflicts to determine which can be done in parallel.

## Input
Array of task objects with at least:
```json
[
  {"id": 1, "files": ["src/api/users.js", "src/models/user.js"]},
  {"id": 2, "files": ["src/components/Login.jsx"]}
]
```

## Output
```json
{
  "conflicts": {
    "1": [],     // Task 1 conflicts with no other tasks
    "2": []      // Task 2 conflicts with no other tasks
  },
  "safe_groups": [
    [1, 2]       // These tasks can be done in parallel
  ],
  "max_parallel": 2
}
```

## Conflict Rules
- Two tasks conflict if they share any files
- Tasks in same safe_group have no shared files
- max_parallel = size of largest safe group

## Algorithm
1. Build file-to-task mapping
2. Find tasks that share files
3. Group non-conflicting tasks
4. Return structured analysis