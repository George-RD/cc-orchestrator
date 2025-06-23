# CC Orchestrator

A minimal AI orchestration framework for Claude Code that uses mermaid diagrams to define behavior and leverages headless mode for clean context management.

## Quick Start

```bash
# Copy the project template to your project
cp -r project-template/* your-project/
cd your-project/

# You now have a complete orchestration framework!
# Start orchestrating:
/orchestrate
```

## What You Get

The `project-template/` directory contains a complete, working example of the framework:

```
your-project/
├── CLAUDE.md                    # Makes Claude the orchestrator
├── .claude/commands/            # Custom commands (/orchestrate, /status, check-conflicts)
├── .cc-orchestrator/            # Framework files (kept separate from your code)
│   ├── specialists/             # AI role definitions
│   └── tasks/                   # Task JSON files
└── src/                         # Your actual project code
```

## How It Works

1. **Self-Contained Commands**: Logic embedded in command files (mermaid + implementation)
2. **Pure Functions**: Headless commands for status and analysis (JSON in/out)
3. **Smart Orchestration**: Checks in-progress before assigning new tasks
4. **Sub-Agent Delegation**: Specialists handle implementation, TDD review
5. **Simple State**: Task status field tracks everything

## Key Features

✅ **< 300 lines total** - Minimal complexity  
✅ **Zero configuration** - Just copy and run  
✅ **Visual programming** - Mermaid diagrams as code  
✅ **Parallel work** - Smart conflict detection  
✅ **State persistence** - Resume anytime via JSON  

## Repository Structure

```
cc-orchestrator/
├── project-template/      # Complete deployable framework (copy this!)
├── docs/                  # Documentation
├── ARCHIVE/               # Old code (will be deleted)
└── README.md             # This file
```

## For Framework Developers

To modify the framework:
1. Read `CLAUDE.md` (framework development guide)
2. Edit files in `project-template/`
3. Test changes by running commands in `project-template/`

## Philosophy

> "Elegant simplicity that works, not complex systems that might work."

This framework prioritizes:
- Clear separation of concerns (MECE)
- Visual understanding over code complexity
- Leveraging Claude Code's native capabilities
- Maintaining context cleanliness

## License

MIT