# Task Sizing Guidelines
# Reusable logic for determining appropriate task size

## Optimal Task Size Criteria

### Duration
- **Sweet spot**: 1-3 days of focused work
- **Too small**: < 2 hours (combine with related work)
- **Too large**: > 5 days (break into subtasks)

### Acceptance Criteria Count
- **Optimal**: 3-7 clear, testable criteria
- **Too few**: < 2 (might be too granular)
- **Too many**: > 10 (split into multiple tasks)

### Complexity Indicators

#### Simple (1 day)
- Single file or component
- No external dependencies
- Clear input/output
- Minimal edge cases
- Well-understood domain

#### Medium (2-3 days)
- Multiple files but single module
- Some integration points
- Moderate edge cases
- Some research needed
- Standard patterns apply

#### Complex (3-5 days max)
- Cross-module changes
- Multiple integration points
- Many edge cases
- Significant unknowns
- Custom solutions needed

## Task Breakdown Rules

### When to Split Tasks

Split if task has:
- Multiple agents required sequentially
- Unclear completion definition
- Mixed concerns (e.g., API + UI + docs)
- Dependencies that could block progress
- Testing that equals implementation time

### How to Split Tasks

#### By Layer
```yaml
original: "User authentication feature"
split_to:
  - "Authentication API endpoints"
  - "Login/Register UI components"  
  - "Session management middleware"
  - "Authentication documentation"
```

#### By Operation
```yaml
original: "CRUD API for users"
split_to:
  - "User creation and validation"
  - "User retrieval and search"
  - "User update operations"
  - "User deletion and cleanup"
```

#### By Complexity
```yaml
original: "Data processing pipeline"
split_to:
  - "Data ingestion interface"
  - "Validation and transformation"
  - "Processing engine core"
  - "Output and reporting"
```

## Size Validation Function

```yaml
validate_task_size:
  inputs:
    - estimated_hours
    - acceptance_criteria_count
    - files_affected
    - agents_required
    
  rules:
    hours_check:
      if: estimated_hours < 2
      then: "TOO_SMALL - Combine with related work"
      
      if: estimated_hours > 40  
      then: "TOO_LARGE - Split into subtasks"
      
      else: "SIZE_OK"
      
    criteria_check:
      if: count < 2
      then: "TOO_SIMPLE - Add more definition"
      
      if: count > 10
      then: "TOO_COMPLEX - Split by concern"
      
      else: "CRITERIA_OK"
      
    agent_check:
      if: multiple_agents_sequential
      then: "SPLIT_BY_AGENT"
      
      else: "AGENT_OK"
```

## Sizing Examples

### Well-Sized Tasks

✅ **"Implement JWT authentication endpoint"**
- 2-3 days effort
- 5 acceptance criteria
- Single agent (backend)
- Clear deliverable

✅ **"Create user profile React component"**
- 2 days effort
- 4 acceptance criteria  
- Single agent (frontend)
- Testable in isolation

### Poorly-Sized Tasks

❌ **"Fix typo in error message"**
- Too small (30 minutes)
- Combine with other fixes

❌ **"Build complete user management system"**
- Too large (2+ weeks)
- Split into: API, UI, Auth, Roles, etc.

❌ **"Update documentation"**
- Too vague
- Split by: API docs, User guide, README

## Auto-Splitting Patterns

For tasks over size threshold:

### Pattern 1: Vertical Slice
```
Large: "Shopping cart feature"
Split:
1. "Cart data model and API"
2. "Add to cart functionality"
3. "Cart display component"
4. "Checkout process"
```

### Pattern 2: Horizontal Layer
```
Large: "API overhaul"
Split:
1. "API design and contracts"
2. "Core endpoints implementation"
3. "Authentication layer"
4. "API documentation"
```

### Pattern 3: Progressive Enhancement
```
Large: "Search feature"
Split:
1. "Basic search functionality"
2. "Advanced filters"
3. "Search suggestions"
4. "Search analytics"
```

---
*Use these guidelines to ensure tasks are appropriately sized for efficient execution.*
