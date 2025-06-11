# Initialize PRD Workflow
# Usage: /prd-init <feature-name>

Initialize a new Product Requirements Document for structured feature development:

## 1. Check Prerequisites

```bash
# Verify feature doesn't already exist
existing_prd=$(find /requirements -name "*${1}*" -type f 2>/dev/null)
if [ -n "$existing_prd" ]; then
    echo "⚠️  Similar PRD may exist: $existing_prd"
    echo "Continue with new PRD? (y/n)"
fi
```

## 2. Create PRD from Template

Copy and customize the PRD template:

```bash
# Copy template
cp /requirements/template.prd.md /requirements/active/${1}.prd.md

# Create version directory
mkdir -p /requirements/active/versions/
cp /requirements/active/${1}.prd.md /requirements/active/versions/${1}.v1.0.prd.md
```

## 3. Guide PRD Completion

Walk through each section of the PRD:

### 3.1 Basic Information
- **Feature Name**: ${1}
- **Version**: 1.0
- **Status**: Draft
- **Author**: [Your name]
- **Created**: $(date -u +"%Y-%m-%dT%H:%M:%SZ")

### 3.2 Problem Statement
❓ **What problem does this solve?**
- Who experiences this problem?
- What is the impact?
- Why solve it now?

### 3.3 Solution Overview
💡 **How will we solve it?**
- High-level approach
- Key components
- Success metrics

### 3.4 Requirements
📋 **What must be built?**

#### Functional Requirements
List specific features and behaviors:
- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

#### Non-Functional Requirements
- Performance: [Specify metrics]
- Security: [Specify requirements]
- Accessibility: [WCAG level]
- Compatibility: [Browsers/devices]

### 3.5 Machine-Readable Configuration
```yaml
# This section is parsed by task creation
agents_required:
  - backend: [true/false]
  - frontend: [true/false]
  - qa: [true/false]
  - docs: [true/false]

priority: [low|normal|high|critical]

estimated_scope:
  size: [small|medium|large]
  sprint_count: [1-4]

dependencies:
  external: []
  internal: []
```

## 4. Validate PRD Completeness

Check that all required sections are filled:

```yaml
validation_checklist:
  - problem_statement: Must be clear and specific
  - success_metrics: Must be measurable  
  - requirements: At least 3 specific items
  - acceptance_criteria: Testable conditions
  - agents_required: At least one selected
  - priority: Must be set
```

## 5. Register PRD

Add to PRD registry:

```json
{
  "prd_id": "${1}",
  "version": "1.0",
  "status": "active",
  "created": "timestamp",
  "task_count": 0,
  "agents_required": ["backend", "frontend"]
}
```

## 6. Next Steps

PRD created successfully! Next actions:

1. **Review PRD**: Ensure all sections are complete
2. **Create tasks**: `/tasks-create ${1}`
3. **Begin work**: `/orchestrate`

### PRD Location:
- Active: `/requirements/active/${1}.prd.md`
- Version: `/requirements/active/versions/${1}.v1.0.prd.md`

---
*PRD initialized. Use `/tasks-create ${1}` when ready to break down into tasks.*
