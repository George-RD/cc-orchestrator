# Claude Template Simplification

## Overview
A simplified AI orchestration framework that maintains power while dramatically reducing complexity. Uses MECE principles and confidence-based delegation for elegant project coordination.

## Quick Start
1. **Copy to your project**: `cp CLAUDE_template.md your-project/CLAUDE.md`
2. **Create a PRD**: Use `.orchestrator/requirements/template.prd.md`
3. **Start orchestrating**: The master orchestrator handles the rest

## Structure
```
├── CLAUDE_template.md (92 lines)      # Master orchestrator
├── .claude/confidence.yaml (20 lines) # Simple autonomy rules
└── .orchestrator/
    ├── shared/coding-ethos.md          # Common principles
    ├── specialists/                    # 4 core specialists (~50 lines each)
    ├── tasks/task-template.json        # Minimal task structure
    └── requirements/template.prd.md    # Simple PRD format
```

## Core Principles
- **MECE**: Mutually Exclusive, Collectively Exhaustive
- **Confidence-Based**: Simple high/medium/low autonomy levels
- **Context Efficient**: Leverages Claude Code's `/compact` feature
- **Progressive Enhancement**: Start minimal, add only proven necessities

## Usage
1. **Create PRD**: Define what you want to build
2. **Run `/orchestrate`**: Smart command detects state and acts
3. **Monitor progress**: Simple confidence-based delegation
4. **Context management**: Use `/compact` between major phases

## Success Metrics
- ✅ Total framework < 500 lines (achieved: ~308 lines)
- ✅ Setup time < 5 minutes
- ✅ Clear separation of concerns (MECE architecture)
- ✅ Maintains resumability across sessions
- ✅ 88% reduction from original complexity

## Philosophy
*Elegant simplicity that works, not complex systems that might work.*

When in doubt, choose the simpler option.