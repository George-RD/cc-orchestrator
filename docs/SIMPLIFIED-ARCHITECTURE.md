# Simplified Architecture

## Key Simplifications Made

### 1. Commands Are Self-Contained
- **Before**: Commands referenced external mermaid files
- **After**: Mermaid diagrams embedded directly in commands
- **Benefit**: Single source of truth, easier to maintain

### 2. Pure Headless Functions
- **Before**: Hybrid explanation/implementation in commands
- **After**: Pure logic that returns JSON
- **Benefit**: Clean separation, reusable functions

### 3. Sub-Agent Delegation
- **Before**: Orchestrator runs verify-tdd command
- **After**: Spawns tdd-reviewer specialist
- **Benefit**: Better separation of concerns

### 4. Smart Task Priority
- **Before**: Complex state management
- **After**: Simple rule - check in-progress before assigning new
- **Benefit**: Prevents overloading, ensures focus

## New Architecture

```
Commands (in .claude/commands/):
├── orchestrate.md      # Contains mermaid logic + implementation
├── status.md           # Pure function: tasks → JSON summary  
└── check-conflicts.md  # Pure function: tasks → conflict analysis

Specialists (in .cc-orchestrator/specialists/):
├── backend.md          # Development specialist
├── frontend.md         # Development specialist
├── qa.md               # Development specialist
├── documentation.md    # Development specialist
└── tdd-reviewer.md     # Review specialist (NEW)
```

## Headless Pattern

```bash
# Status check
STATUS=$(cat .claude/commands/status.md | claude -p --output-format json)

# Conflict check  
CONFLICTS=$(echo "$TASKS" | claude -p < .claude/commands/check-conflicts.md)
```

## Benefits

1. **Maintainable**: Logic and implementation together
2. **Testable**: Pure functions with clear inputs/outputs
3. **Scalable**: Easy to add new specialists or commands
4. **Simple**: No external dependencies or complex references