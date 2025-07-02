# Git Sync Check

Analyze git branches and commit history to understand task progress.

## Purpose
Function that examines git state and returns structured data about task branches.

## Output Format
```json
{
  "task_branches": {
    "task-1-backend": {
      "exists": true,
      "commits_ahead": 5,
      "last_commit": {
        "sha": "abc123",
        "message": "Add failing test for user creation",
        "timestamp": "2024-01-01T12:00:00Z"
      },
      "tdd_pattern_detected": true,
      "commit_pattern": ["test", "implement", "test", "implement", "refactor"]
    }
  },
  "orphaned_branches": ["task-99-frontend"],
  "main_branch": "main"
}
```

## Analysis Logic
1. List all branches matching pattern `task-{id}-{type}`
2. For each task branch:
   - Count commits ahead of main
   - Get last commit info
   - Analyze commit history for TDD pattern
   - Detect commit types: test/implement/refactor
3. Identify orphaned branches (no matching task JSON)
4. Return structured data for status reconciliation

## Git Commands Used
- `git branch -a` - List all branches
- `git rev-list --count main..task-X` - Count commits ahead
- `git log -1 --format="%H|%s|%aI" task-X` - Last commit info
- `git log --format="%s" main..task-X` - Commit messages for pattern analysis

## TDD Pattern Detection
Looks for commit message patterns:
- "test" → Test commit
- "implement" or "add" (without "test") → Implementation
- "refactor" → Refactoring
Valid TDD: test → implement → (optional refactor) → repeat