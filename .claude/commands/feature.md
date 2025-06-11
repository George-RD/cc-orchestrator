# Feature Development Command
# Usage: /feature <feature-name>

Create a comprehensive feature development workflow for ${1}:

1. First, check if a PRD exists for this feature in /.orchestrator/requirements/active/
2. If no PRD exists, help me create one using /.orchestrator/requirements/template.prd.md
3. Once PRD is ready, break down the feature into tasks following the PRD structure
4. Create a task plan with dependencies and agent assignments
5. Begin implementation following TDD workflow from /.claude/workflows/tdd-workflow.yaml

For each component:
- Write tests first (no mock implementations)
- Implement minimal code to pass tests
- Refactor while keeping tests green
- Document as you go

Track progress and maintain alignment with PRD acceptance criteria throughout.
