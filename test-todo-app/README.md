# Example Project with CC Orchestrator

This shows how a project looks with the CC Orchestrator framework deployed.

## Project Structure

```
project/
├── CLAUDE.md                    # Main orchestrator instructions
├── .claude/
│   └── commands/                # Commands (self-contained logic)
│       ├── orchestrate.md       # Main loop with embedded mermaid
│       ├── status.md            # Pure headless status function
│       └── check-conflicts.md   # Pure headless conflict detector
├── .cc-orchestrator/            
│   ├── specialists/             # AI role definitions
│   │   ├── backend.md
│   │   ├── frontend.md
│   │   ├── qa.md
│   │   ├── documentation.md
│   │   └── tdd-reviewer.md      # Reviews TDD compliance
│   └── tasks/                   # Task JSON files
│       ├── task-1.json
│       └── task-template.json
└── src/                         # Your actual code
```

## How It Works

1. **Start**: Run `/orchestrate`
2. **Status Check**: Orchestrator calls status.md via headless mode
3. **Smart Priority**: 
   - Monitor in-progress work first
   - Review completed work next
   - Only assign new if nothing in progress
4. **Delegation**: Spawns specialists as sub-agents
5. **Quality**: TDD reviewer validates all work

## Key Design Principles

### Commands Are Self-Contained
- `orchestrate.md` contains its mermaid logic
- `status.md` is a pure function (headless)
- `check-conflicts.md` is a pure function (headless)

### Headless Pattern
```bash
# Get status as JSON
cat .claude/commands/status.md | claude -p --output-format json

# Check conflicts
echo '[{"id":1,"files":["a.js"]}]' | claude -p < .claude/commands/check-conflicts.md
```

### Sub-Agent Pattern
- Orchestrator spawns specialists via Task tool
- TDD verification delegated to tdd-reviewer specialist
- Each specialist updates their task JSON

## Usage

Just run:
```
/orchestrate
```

The orchestrator handles everything else automatically.