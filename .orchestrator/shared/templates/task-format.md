# Task Template

## Task Identification
```yaml
task_id: [UUID]
created_at: [ISO8601]
created_by: [agent_name]
priority: low|normal|high|critical
type: feature|bugfix|refactor|test|docs|analysis
```

## Task Description
**Title**: [Brief, descriptive title]

**Objective**: [Clear statement of what needs to be accomplished]

**Background**: [Context and why this task is needed]

## Requirements
### Functional Requirements
- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

### Non-Functional Requirements
- Performance: [Specific metrics]
- Security: [Security considerations]
- Accessibility: [WCAG level required]
- Browser Support: [List of browsers]

## Acceptance Criteria
- [ ] Criterion 1 (measurable)
- [ ] Criterion 2 (testable)
- [ ] Criterion 3 (observable)

## Technical Specifications
### Input
```yaml
format: [expected input format]
validation: [validation rules]
example: |
  [example input]
```

### Output
```yaml
format: [expected output format]
schema: [output schema/structure]
example: |
  [example output]
```

### Dependencies
- External: [External services, APIs, libraries]
- Internal: [Other components, modules]
- Data: [Data sources, databases]

## Constraints
- Time: [Deadline or time constraints]
- Technical: [Technical limitations]
- Business: [Business rules or constraints]
- Resources: [Resource limitations]

## Implementation Approach
### Recommended Strategy
[High-level approach to implementing this task]

### Alternative Approaches
1. **Option A**: [Description]
   - Pros: [Advantages]
   - Cons: [Disadvantages]

2. **Option B**: [Description]
   - Pros: [Advantages]
   - Cons: [Disadvantages]

## Risk Assessment
| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| [Risk 1] | Low/Medium/High | Low/Medium/High | [Mitigation strategy] |
| [Risk 2] | Low/Medium/High | Low/Medium/High | [Mitigation strategy] |

## Testing Strategy
### Unit Tests
- [ ] Test case 1
- [ ] Test case 2

### Integration Tests
- [ ] Test case 1
- [ ] Test case 2

### Edge Cases
- [ ] Edge case 1
- [ ] Edge case 2

## Documentation Needs
- [ ] API documentation
- [ ] User guide updates
- [ ] Code comments
- [ ] Architecture diagrams

## Estimated Effort
```yaml
analysis: [hours/points]
implementation: [hours/points]
testing: [hours/points]
documentation: [hours/points]
total: [hours/points]
complexity: simple|medium|complex
```

## Success Metrics
- Metric 1: [How to measure]
- Metric 2: [How to measure]
- Metric 3: [How to measure]

## Communication
### Stakeholders
- [Stakeholder 1]: [Interest/Concern]
- [Stakeholder 2]: [Interest/Concern]

### Update Frequency
- Progress updates: [Daily/Weekly/On completion]
- Blockers: [Immediate notification]

## Task Delegation
### Primary Agent
- Agent: [backend|frontend|qa|docs]
- Rationale: [Why this agent is best suited]

### Supporting Agents
- Agent: [agent_name]
  - Role: [What they'll contribute]
- Agent: [agent_name]
  - Role: [What they'll contribute]

## Definition of Done
- [ ] All acceptance criteria met
- [ ] Tests written and passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Deployed to staging/production
- [ ] Stakeholders notified

## Notes
[Any additional information, clarifications, or context]

---

## Task Execution Log
### Status Updates
| Timestamp | Agent | Status | Notes |
|-----------|-------|--------|-------|
| [ISO8601] | [agent] | started | [notes] |
| [ISO8601] | [agent] | in_progress | [notes] |
| [ISO8601] | [agent] | blocked | [blocker details] |
| [ISO8601] | [agent] | completed | [summary] |

### Decisions Made
| Decision | Rationale | Made By | Timestamp |
|----------|-----------|---------|-----------|
| [decision] | [why] | [agent] | [ISO8601] |

### Lessons Learned
- [Lesson 1]
- [Lesson 2]
