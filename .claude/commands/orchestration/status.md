# Task Status Overview Command  
# Usage: /status [filter]

Display current task status with intelligent formatting and minimal context usage:

## 1. Load Registry Summary

First, load only the summary section to minimize context:

Read from /.orchestrator/tasks/registry.json → summary section only


## 2. Status Display Options

### Default View (no filter):
Show active tasks and recent updates only:

```
📊 Task Status Overview
═══════════════════════════════════════════

🔵 Active Tasks (${active_count})
────────────────────────────────
• task-auth-001: JWT implementation [backend] - 75% complete
  └─ Last update: 2 hours ago - "Implemented token generation"
• task-auth-002: Login UI [frontend] - blocked by auth-001  
  └─ Waiting for: API endpoints
• task-docs-001: API documentation [docs] - 30% complete
  └─ Last update: Yesterday - "Documented auth endpoints"

🟡 In Review (${review_count})
────────────────────────────────
• task-data-003: Database migrations - READY FOR CODE REVIEW
• task-test-001: Integration tests - AWAITING QA VALIDATION

📈 Quick Stats
────────────────────────────────
• Total Active: ${active} tasks
• Blocked: ${blocked} tasks  
• Completed Today: ${completed_today}
• Weekly Velocity: ${velocity} tasks/week
• Next Milestone: ${milestone_name} (${milestone_progress}%)
```

### Filtered Views:

#### `/status --blocked`
Show only blocked tasks with reasons:
```
🚧 Blocked Tasks
════════════════════════════════════
• task-auth-002: Login UI
  ⚠️  Blocked by: task-auth-001 (75% complete)
  📝 Blocker: Waiting for authentication endpoints
  ⏱️  Blocked since: 2025-01-14

• task-deploy-001: Production deployment  
  ⚠️  Blocked by: All tests must pass
  📝 Blocker: Integration tests failing
  ⏱️  Blocked since: 2025-01-13
```

#### `/status --review`
Show tasks ready for review:
```
✅ Ready for Review
════════════════════════════════════
• task-api-001: REST endpoints [backend]
  📋 Type: Code Review
  👤 Suggested Reviewer: Senior Backend
  📁 Files: 12 changed, +450 -120 lines
  
• task-ui-003: Dashboard component [frontend]
  📋 Type: UX Review  
  👤 Suggested Reviewer: Design Team
  🎨 Preview: Available at staging-preview-123
```

#### `/status --completed`
Show recently completed (last 7 days):
```
✅ Recently Completed
════════════════════════════════════
• task-setup-001: Project initialization
  ✓ Merged: commit abc123 - 3 days ago
  
• task-config-001: Environment setup
  ✓ Merged: commit def456 - 5 days ago
```

#### `/status <prd-id>`
Show all tasks for specific PRD:
```
📋 Tasks for: auth-system
════════════════════════════════════
Progress: ████████░░ 80% (4 of 5 tasks)

✅ Completed:
  • task-auth-design: System design
  • task-auth-db: Database schema
  • task-auth-001: JWT implementation
  
🔵 Active:
  • task-auth-002: Login UI [frontend]
  
🔜 Upcoming:
  • task-auth-test: E2E testing
```

## 3. Registry Management

For large projects, implement smart loading:

```yaml
registry_loading:
  small_project: # < 50 tasks
    - Load full active and blocked tasks
    
  medium_project: # 50-200 tasks  
    - Load only task IDs and status
    - Fetch details on demand
    
  large_project: # > 200 tasks
    - Load summary counts only
    - Use filters to drill down
    - Archive completed > 30 days
```

## 4. Performance Optimization

To prevent context overload:
1. Never load full task details unless requested
2. Use task IDs for reference
3. Load work_log only when drilling into specific task
4. Archive old tasks to `/.orchestrator/tasks/archive/`

## 5. Export Options

For external tracking:
- `/status --export-csv` - Simple CSV for spreadsheets
- `/status --export-json` - Full data for integrations
- `/status --export-markdown` - For reports

---
*Intelligent status display with context-aware loading*
