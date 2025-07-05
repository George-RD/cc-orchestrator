# /worktree-init

Initialize git worktrees for parallel specialist development.

## Purpose
Creates separate working directories for each specialist type, allowing parallel development without branch conflicts.

## Process
1. Create `.worktrees/` directory structure
2. Initialize git worktrees for each specialist type
3. Add `.worktrees/` to `.gitignore`

## Implementation

```bash
# Ensure we're in the project root
if [ ! -d ".cc-orchestrator" ]; then
    echo "❌ Error: Must be run from project root (directory containing .cc-orchestrator)"
    exit 1
fi

# Clean up any existing worktrees
echo "🧹 Cleaning up existing worktrees..."
rm -rf .worktrees/
git worktree prune

# Create worktrees directory
mkdir -p .worktrees

# Initialize worktrees for each specialist type
echo "🔧 Setting up git worktrees for specialists..."

# Store current directory to prevent recursive worktree creation
PROJECT_ROOT=$(pwd)

# Backend specialist worktree
git worktree add .worktrees/backend HEAD
git -C .worktrees/backend sparse-checkout init
echo -e "/*\n!.cc-orchestrator\n!.claude" | git -C .worktrees/backend sparse-checkout set --stdin
echo "✅ Backend worktree created at .worktrees/backend"

# Frontend specialist worktree  
git worktree add .worktrees/frontend HEAD
git -C .worktrees/frontend sparse-checkout init
echo -e "/*\n!.cc-orchestrator\n!.claude" | git -C .worktrees/frontend sparse-checkout set --stdin
echo "✅ Frontend worktree created at .worktrees/frontend"

# QA specialist worktree
git worktree add .worktrees/qa HEAD
git -C .worktrees/qa sparse-checkout init
echo -e "/*\n!.cc-orchestrator\n!.claude" | git -C .worktrees/qa sparse-checkout set --stdin
echo "✅ QA worktree created at .worktrees/qa"

# Documentation specialist worktree
git worktree add .worktrees/documentation HEAD
git -C .worktrees/documentation sparse-checkout init
echo -e "/*\n!.cc-orchestrator\n!.claude" | git -C .worktrees/documentation sparse-checkout set --stdin
echo "✅ Documentation worktree created at .worktrees/documentation"

# Add .worktrees/ to .gitignore if not already present
if ! grep -q "^\.worktrees/" .gitignore 2>/dev/null; then
    echo "" >> .gitignore
    echo "# Git worktrees for parallel development" >> .gitignore
    echo ".worktrees/" >> .gitignore
    echo "✅ Added .worktrees/ to .gitignore"
else
    echo "⚠️  .worktrees/ already in .gitignore"
fi

echo ""
echo "🎉 Worktree setup complete!"
echo ""
echo "Specialists will now work in isolated environments:"
echo "  📂 Backend:       .worktrees/backend/"
echo "  📂 Frontend:      .worktrees/frontend/" 
echo "  📂 QA:            .worktrees/qa/"
echo "  📂 Documentation: .worktrees/documentation/"
echo ""
echo "Key features:"
echo "  • Task files (.cc-orchestrator/) excluded from worktrees"
echo "  • Orchestration commands (.claude/) excluded from worktrees"
echo "  • Specialists focus purely on code implementation"
echo "  • Single source of truth for tasks in main repository"
echo ""
echo "The TDD reviewer will continue to work in the main repository directory."
echo "All git branches created by specialists will be available for review and merging."
```

## Directory Structure Created
```
project-root/
├── .worktrees/
│   ├── backend/          # Backend specialist workspace
│   ├── frontend/         # Frontend specialist workspace  
│   ├── qa/               # QA specialist workspace
│   └── documentation/    # Documentation specialist workspace
├── .cc-orchestrator/     # Main orchestrator (unchanged)
└── .gitignore           # Updated with .worktrees/
```

## Benefits
- ✅ **Parallel Development**: Multiple specialists can work simultaneously
- ✅ **No Branch Conflicts**: Each specialist works in their own directory
- ✅ **Shared Git History**: All branches available for review and merging
- ✅ **Clean Main Repo**: Worktrees are git-ignored
- ✅ **Minimal Changes**: Existing workflow preserved