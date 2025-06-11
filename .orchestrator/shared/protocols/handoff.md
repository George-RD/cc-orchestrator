# Task Handoff Protocol

## Overview
Defines how agents transfer work between each other with complete context preservation.

## Handoff Package Structure

```yaml
handoff:
  id: uuid
  timestamp: ISO8601
  from_agent: string
  to_agent: string
  task:
    id: uuid
    title: string
    description: string
    priority: low|normal|high|critical
    deadline: ISO8601 (optional)
  
  context:
    background: string
    current_state: object
    completed_work:
      - action: string
        result: string
        timestamp: ISO8601
    dependencies:
      - type: string
        details: object
    constraints:
      - type: string
        requirement: string
  
  deliverables:
    expected:
      - name: string
        type: string
        criteria: string
    completed:
      - name: string
        location: string
        validation: passed|failed|pending
  
  next_steps:
    - action: string
      rationale: string
      estimated_effort: string
  
  metadata:
    confidence: 0.0-1.0
    complexity: simple|medium|complex
    risks:
      - description: string
        mitigation: string
```

## Handoff Types

### Sequential Handoff
One agent completes their portion and passes to the next:
```yaml
type: sequential
flow:
  - backend: implement API
  - frontend: create UI
  - qa: test integration
  - docs: update guides
```

### Parallel Handoff
Multiple agents work simultaneously:
```yaml
type: parallel
coordination:
  sync_points:
    - after: [backend.api, frontend.components]
      action: integration_test
  merge_strategy: coordinate_through_orchestrator
```

### Conditional Handoff
Next agent depends on outcome:
```yaml
type: conditional
rules:
  - if: tests_pass
    then: docs
  - if: tests_fail
    then: backend
  - if: performance_issue
    then: backend + frontend
```

## Context Preservation

### Required Context Elements
1. **Problem Statement**: Original requirement
2. **Solution Approach**: Chosen implementation strategy
3. **Decision History**: Key choices and rationale
4. **Technical Details**: APIs, schemas, configurations
5. **Test Results**: What's been validated
6. **Known Issues**: Bugs, limitations, tech debt

### Context Enrichment
Each agent should add:
- Work performed
- Decisions made
- Discoveries
- Recommendations
- Warnings

## Handoff Validation

### Pre-Handoff Checklist
- [ ] All expected deliverables complete
- [ ] Tests passing (if applicable)
- [ ] Documentation updated
- [ ] Known issues documented
- [ ] Next steps clearly defined

### Acceptance Criteria
Receiving agent validates:
1. Context completeness
2. Deliverable quality
3. Dependency availability
4. Clear action items

## Recovery Procedures

### Incomplete Handoff
```yaml
if: handoff_incomplete
then:
  - acknowledge_receipt
  - identify_gaps
  - request_clarification
  - proceed_with_available_info
```

### Handoff Rejection
```yaml
if: critical_issues
then:
  - document_issues
  - return_to_sender
  - suggest_remediation
  - escalate_if_needed
```

## Best Practices

1. **Explicit is better than implicit**
   - Document all assumptions
   - List all dependencies
   - Clarify all ambiguities

2. **Progressive enhancement**
   - Each handoff adds value
   - Never remove context
   - Build on previous work

3. **Fail gracefully**
   - Always acknowledge receipt
   - Communicate issues early
   - Provide workarounds

4. **Maintain traceability**
   - Link to original requirements
   - Reference previous handoffs
   - Track decision evolution

## Example Handoff

```yaml
handoff:
  id: "h-123456"
  timestamp: "2025-01-15T10:00:00Z"
  from_agent: "backend"
  to_agent: "frontend"
  
  task:
    id: "t-789"
    title: "User Authentication UI"
    description: "Create login/register forms for auth system"
    priority: high
    deadline: "2025-01-20T17:00:00Z"
  
  context:
    background: "Implementing JWT-based authentication"
    current_state:
      api_endpoints:
        - POST /api/v1/auth/login
        - POST /api/v1/auth/register
        - POST /api/v1/auth/refresh
      response_format: "JSON with token and user object"
    completed_work:
      - action: "Implemented auth endpoints"
        result: "All endpoints tested and documented"
        timestamp: "2025-01-15T09:30:00Z"
  
  deliverables:
    expected:
      - name: "Login form component"
        type: "React component"
        criteria: "Validates input, handles errors, responsive"
    completed:
      - name: "Auth API"
        location: "/src/api/auth.js"
        validation: passed
  
  next_steps:
    - action: "Create login form with email/password fields"
      rationale: "Users need to authenticate"
    - action: "Implement form validation"
      rationale: "Prevent invalid submissions"
    - action: "Add error handling for 401/403 responses"
      rationale: "Guide users on auth failures"
  
  metadata:
    confidence: 0.95
    complexity: medium
    risks:
      - description: "Token storage security"
        mitigation: "Use httpOnly cookies or secure storage"
```

## Monitoring and Metrics

Track handoff quality:
- Acceptance rate
- Rework frequency  
- Time to acceptance
- Context completeness score
- Handoff velocity
