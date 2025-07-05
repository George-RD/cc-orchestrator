# CC Orchestrator Template

A minimal AI orchestration framework for Claude Code that leverages headless mode for clean context management and self-contained command logic.

## Using This Template

### Quick Start with GitHub Templates

1. Click the "Use this template" button on GitHub
2. Name your new repository
3. Clone your new repository
4. Add task JSON files to `.cc-orchestrator/tasks/` (see task creation section below)
5. Start orchestrating with `/orchestrate`

### Manual Setup

If not using GitHub's template feature:

```bash
git clone https://github.com/George-RD/cc-orchestrator your-project-name
cd your-project-name

# Initialize git worktrees for parallel development
/worktree-init

# Add your task JSON files to .cc-orchestrator/tasks/
# See "Create Your Tasks" section below for details

# Then start orchestrating
/orchestrate
```

## Recent Updates

🚀 **Git Worktree Support** (July 4, 2025)
- Added `/worktree-init` command for true parallel development
- Multiple specialists can now work simultaneously without conflicts
- Isolated workspaces prevent branch switching issues

📋 **GitHub Template** (July 2, 2025)  
- Converted to GitHub template for easy project creation
- Added comprehensive documentation and Next Steps section
- Streamlined setup process

🔧 **Framework Improvements** (June 24, 2025)
- Enhanced git synchronization and conflict detection
- Updated task templates and orchestration documentation
- Improved framework consistency

## What You Get

This template provides a complete AI orchestration framework:

```
your-project/
├── CLAUDE.md                    # Makes Claude the orchestrator
├── .claude/commands/            # Custom commands (/orchestrate, /status, check-conflicts)
├── .cc-orchestrator/            # Framework files (kept separate from your code)
│   ├── specialists/             # AI role definitions
│   └── tasks/                   # Task JSON files
└── src/                         # Your actual project code (create as needed)
```

## How It Works

1. **Self-Contained Commands**: Logic embedded in command files (mermaid + implementation)
2. **Pure Functions**: Headless commands for status and analysis (JSON in/out)
3. **Smart Orchestration**: Checks in-progress before assigning new tasks
4. **Sub-Agent Delegation**: Specialists handle implementation, TDD review
5. **Simple State**: Task status field tracks everything

## Getting Started with Your Project

### 1. Customize This README

Replace this content with your project-specific documentation:
- Project name and description
- Installation instructions
- Usage examples
- Contributing guidelines

### 2. Create Your Tasks

The orchestration framework uses JSON files to define development tasks. Here's how to get started:

#### Option A: Manual Task Creation

Add task JSON files to `.cc-orchestrator/tasks/` following the template:

```json
{
  "id": "task-001",
  "name": "Your task name",
  "description": "What needs to be done",
  "specialist": "backend|frontend|qa|documentation",
  "status": "todo",
  "requirements": ["List of requirements"],
  "acceptance_criteria": ["How to verify completion"]
}
```

#### Option B: Use Claude Taskmaster (Recommended)

For a more streamlined task creation experience, we recommend using [Claude Taskmaster](https://github.com/eyaltoledano/claude-task-master) - an AI-powered task management system that integrates seamlessly with Claude Code.

Taskmaster can help you:
- Generate properly formatted task JSON files automatically
- Break down complex features into manageable tasks
- Ensure tasks follow best practices for AI delegation
- Maintain consistent task structure across your project

### 3. Start Development

Once you have your task JSON files in `.cc-orchestrator/tasks/`:

```bash
# Initialize git worktrees for parallel development (first time only)
/worktree-init

# Start orchestrating
/orchestrate
```

The orchestrator will:
- Automatically detect and load your task files
- Manage development tasks in the optimal order
- Coordinate AI specialists for parallel work in isolated worktrees
- Ensure code quality through TDD review
- Keep documentation updated throughout development

## Key Features

✅ **Minimal complexity** - Framework focused on simplicity  
✅ **Zero configuration** - Just copy and run  
✅ **Self-contained logic** - Commands contain their implementation  
✅ **Parallel work** - Git worktrees enable true parallel development  
✅ **State persistence** - Resume anytime via JSON  

## Framework Commands

- `/orchestrate` - Start the orchestration loop
- `/status` - Check current task statuses
- `/check-conflicts` - Verify no file conflicts between tasks
- `/git-sync-check` - Ensure git repository is in sync
- `/search-tips` - Get help with code search
- `/worktree-init` - Initialize git worktrees for parallel development
- `/init-claude` - Restore CLAUDE.md orchestrator configuration

## Customizing Specialists

Modify specialist behaviors by editing files in `.cc-orchestrator/specialists/`:
- `backend.md` - APIs, databases, business logic
- `frontend.md` - UI, UX, client-side code
- `qa.md` - Testing, validation, quality
- `documentation.md` - Docs, examples, guides
- `tdd-reviewer.md` - Test-driven development verification

## Philosophy

> "Elegant simplicity that works, not complex systems that might work."

This framework prioritizes:
- Clear separation of concerns (MECE)
- Visual understanding over code complexity
- Leveraging Claude Code's native capabilities
- Maintaining context cleanliness

## Next Steps

### Planned Development

#### 1. Claude Taskmaster Integration
- Build adapter to convert Taskmaster-generated JSON to CC Orchestrator task format
- Direct CLI integration: Use Claude Code hooks to call Taskmaster commands directly
- Hook-based workflow: Trigger Taskmaster task generation via pre/post orchestration hooks
- Explore MCP integration once Taskmaster MCP support stabilizes
- Support seamless workflow from Taskmaster task generation to orchestration execution

#### 2. Visual Project Dashboard
- Create web-based visualizer to parse and display project structure from task JSON files
- Show task dependencies, status, and specialist assignments in an interactive graph
- Real-time updates as orchestration progresses

#### 3. Claude Code Hooks Implementation
- Leverage Claude Code's new hooks functionality to streamline context usage
- Replace verbose tool calls with efficient bash commands via hooks
- Implement pre/post-task hooks for automatic validation and formatting
- Use hooks to enforce consistent git operations and testing workflows

#### 4. Enhanced Headless Mode Usage
- Explore additional headless mode applications for automation
- Build command-line utilities for common orchestration patterns
- Create reusable headless scripts for CI/CD integration

### Contributing

Contributions welcome! Priority areas:
- Taskmaster JSON adapter development
- Visual dashboard implementation
- Hook templates for common workflows
- Documentation and examples

## License

MIT