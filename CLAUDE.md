# CLAUDE.md - CC Orchestrator Framework Development

This file guides Claude Code sessions working on the CC Orchestrator framework itself.

## Framework Overview

CC Orchestrator is a minimal AI orchestration framework using:
- **Self-contained commands** with embedded logic
- **Pure headless functions** for status and analysis
- **Sub-agent delegation** via Task tool
- **Simple task management** with JSON status field

## Repository Structure

```
/cc-orchestrator/
├── project-template/            # 🎯 Complete deployable framework
│   ├── CLAUDE.md               # User's orchestrator
│   ├── .claude/commands/       # Self-contained commands
│   │   ├── orchestrate.md      # Main loop + mermaid
│   │   ├── status.md           # Pure status function
│   │   └── check-conflicts.md  # Pure conflict checker
│   └── .cc-orchestrator/       
│       ├── specialists/        # AI roles (including tdd-reviewer)
│       └── tasks/              # Task JSON files
│
├── docs/                        # Documentation
├── ARCHIVE/                     # Old code (delete after stable)
└── README.md                    # User guide
```

## Key Design Principles

### 1. Commands Are Logic
Each command file contains:
- Mermaid diagram (if flow logic)
- Implementation referencing the diagram
- No external dependencies

### 2. Headless Functions
Pure functions that:
- Take input (stdin or parameters)
- Return JSON output
- No side effects or explanations

### 3. Sub-Agent Pattern
- Orchestrator delegates specialized work
- TDD verification → tdd-reviewer specialist
- Each specialist has single responsibility

## Development Workflow

```bash
cd project-template/

# Test orchestration
/orchestrate

# Test headless functions
cat .claude/commands/status.md | claude -p --output-format json
```

## Common Modifications

### Change Orchestration Logic
1. Edit mermaid in `orchestrate.md`
2. Update implementation to match
3. Test full cycle

### Add New Specialist
1. Create `.cc-orchestrator/specialists/role.md`
2. Update orchestrator assignment logic
3. Test task delegation

### Modify Status Logic
1. Edit `status.md` analysis rules
2. Test JSON output format
3. Verify orchestrator uses it correctly

## Framework Philosophy

- **Simple**: Each piece does one thing well
- **Visual**: Logic visible in mermaid
- **Testable**: Pure functions, clear contracts
- **Maintainable**: Self-contained components