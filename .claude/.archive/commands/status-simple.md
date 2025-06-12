# /status - Lightweight Status Command

## Purpose
Quick project status without heavy file parsing.

## Display Information
- **Active Tasks**: Count and titles only
- **Blocked Tasks**: Count with brief reasons
- **Completed Today**: Recent completions
- **Next Actions**: Clear next steps

## Implementation
```bash
# Simple file counts (no JSON parsing)
echo "Active tasks: $(ls .orchestrator/tasks/active/ 2>/dev/null | wc -l)"
echo "Blocked tasks: $(ls .orchestrator/tasks/blocked/ 2>/dev/null | wc -l)"
echo "Completed: $(ls .orchestrator/tasks/completed/ 2>/dev/null | wc -l)"

# Show only task titles from filenames
ls .orchestrator/tasks/active/ 2>/dev/null | sed 's/\.json$//' | head -5
```

## Context Budget
Minimal - avoids loading full task JSONs to preserve context for actual work.