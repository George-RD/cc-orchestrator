# Repository Structure

## For Users (Just Want to Use the Framework)

Look at `project-template/` - this is exactly what your project will look like:

```
project-template/
├── CLAUDE.md                 # 🧠 Makes Claude the orchestrator
├── .claude/commands/         # 🎮 Custom commands
├── specialists/              # 👥 AI worker roles  
├── tasks/                    # 📋 Your tasks as JSON
└── src/                      # 💻 Your actual code
```

**To use**: Copy everything from `project-template/` to your project.

## For Framework Developers

```
cc-orchestrator/
├── project-template/         # What users copy (complete example)
│
├── framework/                # Core logic
│   ├── orchestrator-flow.mmd # Main state machine
│   ├── task-assignment.mmd   # Parallel work logic
│   ├── tdd-verification.mmd  # Test enforcement
│   └── headless-specs/       # Headless patterns
│
├── templates/                # Source files
│   ├── CLAUDE-project.md     # Orchestrator template
│   ├── specialists/          # Role templates
│   └── task-template.json    # Task structure
│
├── commands/                 # Command sources
│   ├── orchestrate.md
│   ├── status.md
│   └── verify-tdd.md
│
├── docs/                     # Documentation
│   ├── ARCHITECTURE.md       # How it works
│   └── refactor/             # Development history
│
└── ARCHIVE/                  # Old code (delete after stable)
```

## Key Files

1. **Start Here**: `project-template/README.md` - See working example
2. **Framework Logic**: `framework/*.mmd` - Visual behavior definitions
3. **Dev Guide**: `CLAUDE.md` - For framework development

## Quick Test

```bash
cd project-template
# This is now a complete working project
# Run /orchestrate to see it work
```