# Archive - Old Framework Structure

This directory contains the old framework structure archived during the refactor.
**These files are kept for reference only and will be deleted after the new framework is proven stable.**

## Archive Contents

### `/orchestrator/` - Old orchestrator structure
- `specialists/` - Original 55-line specialist definitions (replaced by 15-line versions)
- `shared/` - Coding ethos and unused patterns
- `templates/` - Original templates (moved to `/templates/`)
- `tasks/` - Example of old task directory structure

### `/claude-commands/` - Old command structure  
- `orchestrate.md` - Original complex orchestration command (replaced by orchestrate-simple.md)
- `orchestration/` - Sub-commands that added complexity:
  - `orch-prd-init.md`
  - `orch-resume.md`
  - `orch-spawn.md`
  - `orch-status.md`
  - `orch-tasks-create.md`

### `/CLAUDE.md.old` - Previous orchestrator template
- 95-line orchestrator for end projects
- Included worktree management
- Complex state handling

## Why These Were Replaced

1. **Worktree Complexity**: Git worktrees added unnecessary complexity
2. **Scattered Logic**: Orchestration logic was spread across multiple files
3. **Heavy Context**: Too much loaded into orchestrator context
4. **No Visual Logic**: Behavior was hidden in code, not visual
5. **Complex State**: Task directories vs simple status field

## What to Reference

If you need to check old logic:
- Task state flow: See `orchestrator/shared/` 
- Specialist details: See `orchestrator/specialists/`
- Command logic: See `claude-commands/orchestrate.md`

## When to Delete

Delete this entire archive when:
- [ ] New framework tested successfully on 3+ projects
- [ ] All useful patterns extracted and documented
- [ ] Team consensus that old structure not needed
- [ ] At least 2 weeks of stable operation

**Target deletion date: [2 weeks after first successful project]**