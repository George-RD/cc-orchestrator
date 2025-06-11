# PRD Evolution Management
# Usage: /prd-evolve <prd-id> <change-type>

Manage PRD updates with full traceability and impact analysis:

## 1. Change Types

### 1.1 Clarification
Minor updates that don't affect implementation:
- Typo fixes
- Better explanations
- Example additions
- Non-functional requirement details

### 1.2 Enhancement  
New features or requirements added:
- Additional endpoints
- New UI components
- Extended functionality
- Performance improvements

### 1.3 Pivot
Major direction changes:
- Architecture changes
- Technology switches
- Scope modifications
- Approach overhaul

## 2. Version Management

### 2.1 Create New Version
```bash
# Determine version bump
current_version=$(grep "version:" /.orchestrator/requirements/active/${1}.prd.md | cut -d: -f2)

case ${2} in
  clarification)
    new_version=$(increment_patch $current_version)  # 1.0.0 → 1.0.1
    ;;
  enhancement)
    new_version=$(increment_minor $current_version)  # 1.0.0 → 1.1.0
    ;;
  pivot)
    new_version=$(increment_major $current_version)  # 1.0.0 → 2.0.0
    ;;
esac

# Create versioned copy
cp /.orchestrator/requirements/active/${1}.prd.md \
   /.orchestrator/requirements/active/versions/${1}.v${new_version}.prd.md
```

### 2.2 Update Changelog
Create/append to `/.orchestrator/requirements/active/${1}.changelog.md`:

```markdown
# ${1} PRD Changelog

## v${new_version} - $(date +%Y-%m-%d)
### Change Type: ${2}

### What Changed
- [Specific changes made]
- [Another change]

### Why Changed  
[Rationale for the changes]

### Impact Analysis
Tasks Affected:
- task-xxx-001: [Impact description]
- task-xxx-002: [Impact description]

Required Actions:
- [ ] Review affected tasks
- [ ] Update acceptance criteria
- [ ] Notify assigned agents
```

## 3. Impact Analysis

### 3.1 Scan Affected Tasks
```javascript
// Pseudo-code for impact analysis
affected_tasks = []
for task in /.orchestrator/tasks/*/*.json:
  if task.prd_id == ${1}:
    if task.prd_version < new_version:
      affected_tasks.add({
        task_id: task.id,
        status: task.status,
        assigned_to: task.assigned_to,
        impact_level: analyze_impact(task, changes)
      })
```

### 3.2 Impact Levels
```yaml
impact_levels:
  none:
    - Changes don't affect this task
    - Continue as planned
    
  minor:
    - Acceptance criteria refinement
    - Documentation updates only
    
  major:
    - Implementation changes required
    - May need partial rework
    
  critical:
    - Complete task redesign needed
    - Current work invalid
```

## 4. Task Notification

For each affected task, add notification to work_log:

```json
{
  "timestamp": "2025-01-15T10:00:00Z",
  "agent": "orchestrator",
  "action": "PRD Updated - Review Required",
  "details": "PRD ${1} updated from v${old} to v${new}",
  "prd_changes": {
    "version": "${new_version}",
    "change_type": "${2}",
    "summary": "[Change summary]",
    "impact": "major",
    "required_actions": [
      "Review new requirements",
      "Update implementation if needed",
      "Revalidate acceptance criteria"
    ]
  }
}
```

## 5. Update Task Registry

```json
{
  "prd_versions": {
    "${1}": {
      "current": "${new_version}",
      "tasks_on_old_version": [${affected_task_ids}],
      "update_required": true
    }
  }
}
```

## 6. Agent Notifications

Based on impact analysis:

```markdown
📢 PRD Update Notification
════════════════════════════════════════════

PRD: ${1}
Version: ${old_version} → ${new_version}
Change Type: ${2}

Affected Tasks:
1. task-xxx-001 [backend] - Major impact
   ⚠️ Implementation changes required
   
2. task-xxx-002 [frontend] - Minor impact
   ℹ️ Update documentation only

3. task-xxx-003 [qa] - No impact
   ✅ Continue as planned

Required Actions:
- Agents with major impact tasks should review immediately
- Update task acceptance criteria where needed
- Document any scope changes in work_log
```

## 7. Orchestrator Guidance

Provide recommendations:

```yaml
recommendations:
  for_in_progress_tasks:
    minor_impact:
      - Continue work
      - Update docs later
      
    major_impact:
      - Pause current work
      - Review changes
      - Estimate rework
      - Update timeline
      
  for_completed_tasks:
    critical_impact:
      - May need to reopen
      - Assess if still valid
      - Plan updates
```

## 8. Output Summary

```
✅ PRD Evolution Complete
════════════════════════════════════════════

PRD: ${1}
Version: ${old} → ${new}
Change Type: ${2}

Files Updated:
- /.orchestrator/requirements/active/${1}.prd.md (current)
- /.orchestrator/requirements/active/versions/${1}.v${new}.prd.md
- /.orchestrator/requirements/active/${1}.changelog.md

Impact Summary:
- Total tasks: ${total_tasks}
- Affected tasks: ${affected_count}
- Critical impact: ${critical_count}
- Major impact: ${major_count}
- Minor impact: ${minor_count}

Next Steps:
1. Review affected tasks: /status ${1} --outdated
2. Update critical tasks first
3. Communicate changes to team
```

## 9. Rollback Capability

If changes prove problematic:

```bash
/prd-rollback ${1} ${version}

# Restores previous version
# Notifies all affected tasks
# Documents rollback reason
```

---
*PRD updated with full traceability. Review affected tasks.*
