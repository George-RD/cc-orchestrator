# Claude Template Simplification

## Overview
A simplified AI orchestration framework that maintains power while dramatically reducing complexity. Uses MECE principles and confidence-based delegation for elegant project coordination.

## Quick Start
1. **Copy to your project**: `cp CLAUDE_template.md your-project/CLAUDE.md`
2. **Create a PRD**: Use `.orchestrator/requirements/template.prd.md`
3. **Start orchestrating**: The master orchestrator handles the rest

## Structure
```
├── CLAUDE_template.md (95 lines)      # Master orchestrator
├── .claude/
│   ├── confidence.yaml (35 lines)     # Simple autonomy rules
│   └── commands/git-workflow.md       # Git worktree management
├── worktrees/                         # Development specialist worktrees
│   ├── backend-work/                  # Backend development branch
│   ├── frontend-work/                 # Frontend development branch
│   ├── qa-work/                       # QA development branch
│   └── docs-work/                     # Documentation branch
│   # DevOps & Security work in main branch (infrastructure/project-wide)
└── .orchestrator/
    ├── shared/coding-ethos.md          # Common principles + git workflow
    ├── specialists/                    # 4 core specialists (~55 lines each)
    ├── tasks/task-template.json        # Task structure with git tracking
    └── requirements/template.prd.md    # Simple PRD format
```

## Core Principles
- **MECE**: Mutually Exclusive, Collectively Exhaustive
- **Confidence-Based**: Simple high/medium/low autonomy levels
- **Context Efficient**: Natural flow through orchestrator delegation
- **Progressive Enhancement**: Start minimal, add only proven necessities

## Usage
1. **Create PRD**: Define what you want to build
2. **Setup git worktrees**: `git worktree add worktrees/{specialist}-work {specialist}-work` (for backend, frontend, qa, docs)
3. **Run `/orchestrate`**: Smart command detects state and delegates to worktrees
4. **Monitor progress**: Each specialist commits regularly in their worktree
5. **Integration**: Orchestrator merges completed features from worktrees to main

## Success Metrics
- ✅ Total framework < 500 lines (achieved: ~308 lines)
- ✅ Setup time < 5 minutes
- ✅ Clear separation of concerns (MECE architecture)
- ✅ Maintains resumability across sessions
- ✅ 88% reduction from original complexity

## Philosophy
*Elegant simplicity that works, not complex systems that might work.*

When in doubt, choose the simpler option.