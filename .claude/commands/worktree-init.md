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

# Create worktrees directory
mkdir -p .worktrees

# Initialize worktrees for each specialist type
echo "🔧 Setting up git worktrees for specialists..."

# Backend specialist worktree
if [ ! -d ".worktrees/backend" ]; then
    git worktree add .worktrees/backend main
    echo "✅ Backend worktree created at .worktrees/backend"
else
    echo "⚠️  Backend worktree already exists"
fi

# Frontend specialist worktree  
if [ ! -d ".worktrees/frontend" ]; then
    git worktree add .worktrees/frontend main
    echo "✅ Frontend worktree created at .worktrees/frontend"
else
    echo "⚠️  Frontend worktree already exists"
fi

# QA specialist worktree
if [ ! -d ".worktrees/qa" ]; then
    git worktree add .worktrees/qa main
    echo "✅ QA worktree created at .worktrees/qa"
else
    echo "⚠️  QA worktree already exists"
fi

# Documentation specialist worktree
if [ ! -d ".worktrees/documentation" ]; then
    git worktree add .worktrees/documentation main
    echo "✅ Documentation worktree created at .worktrees/documentation"
else
    echo "⚠️  Documentation worktree already exists"
fi

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