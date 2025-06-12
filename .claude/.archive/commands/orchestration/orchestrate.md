# Intelligent Orchestration Command
# Usage: /orchestrate

Analyze repository state and provide intelligent guidance:

## 1. State Analysis

First, I'll check the current repository state:

```bash
# Check for existing tasks
active_tasks=$(ls /.orchestrator/tasks/active/ 2>/dev/null | wc -l)
blocked_tasks=$(ls /.orchestrator/tasks/blocked/ 2>/dev/null | wc -l)
completed_recent=$(find /.orchestrator/tasks/completed/ -type f -mtime -7 2>/dev/null | wc -l)

# Check for PRDs
active_prds=$(ls /.orchestrator/requirements/active/*.prd.md 2>/dev/null | wc -l)

# Check git status
uncommitted_changes=$(git status --porcelain 2>/dev/null | wc -l)

# Load registry summary only (not full task list)
registry_summary=$(jq '.summary' /.orchestrator/tasks/registry.json 2>/dev/null)
```

## 2. Smart Recommendations

Based on the analysis, I'll provide contextual guidance:

### If uncommitted changes exist:
⚠️  **Uncommitted changes detected** 
   - Review changes: `git status`
   - Commit if complete: `git add . && git commit -m "message"`
   - Stash if switching context: `git stash`

### If active tasks exist:
📋 **${active_tasks} active tasks found**
   - View status: `/status`
   - Resume work: `/orchestrate-resume`
   - Check blockers: `/status --blocked`

### If blocked tasks exist:
🚧 **${blocked_tasks} tasks are blocked**
   - Review blockers: `/status --blocked`
   - May need to complete dependencies first

### If PRD exists but no tasks:
📄 **PRD found without tasks**
   - Create tasks: `/tasks-create [prd-id]`
   - Review PRD: `/prd-status [prd-id]`

### If no work exists:
🚀 **Ready to start new work**
   - Initialize new feature: `/prd-init [feature-name]`
   - Or create quick task: `/task-quick [description]`

## 3. Current Project Summary

Based on registry summary:
- Total tasks: ${total}
- Active: ${active} | Blocked: ${blocked} | In Review: ${review}
- Recent velocity: ${completed_recent} tasks in last 7 days

## 4. Suggested Next Action

**Recommended command**: `[specific command based on state]`

## 5. Quick Help

Common commands:
- `/status` - View current task status
- `/orchestrate-resume` - Resume active work
- `/prd-init <name>` - Start new feature
- `/task-history` - View completed work
- `/help orchestration` - Full command list

---
*Smart orchestration based on repository state analysis*
