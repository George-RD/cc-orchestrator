# Product Requirements Document: [Feature Name]

## Document Metadata
```yaml
version: 1.0.0
status: draft
last_updated: YYYY-MM-DD
author: [Author Name]
```

## Machine-Readable Configuration
```yaml
agents:
  - backend: required
  - frontend: required
  - qa: required
  - docs: optional
  - devops: optional  # required for deployment/infrastructure changes
  - security: optional  # required for security-sensitive features
constraints:
  security_level: high  # low, medium, high, critical
  performance_target: <100ms
  accessibility: WCAG-AA
  browser_support: ["chrome", "firefox", "safari", "edge"]
  compliance_requirements: []  # GDPR, PCI, SOX, HIPAA
  deployment_environments: ["dev", "staging", "production"]
complexity: medium  # simple, medium, complex
estimated_effort: 5  # story points or days
```

## Problem Statement
<!-- Describe the customer problem we're solving -->

## Solution Overview
<!-- High-level description of the proposed solution -->

## User Stories
<!-- User-focused requirements -->
- As a [user type], I want to [action] so that [benefit]
- As a [user type], I want to [action] so that [benefit]

## Acceptance Criteria
<!-- Measurable success criteria -->
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Technical Requirements

### Backend Requirements
```yaml
endpoints:
  - method: POST
    path: /api/v1/resource
    description: Create new resource
    auth: required
    validation:
      - field: required
      - field2: optional
```

### Frontend Requirements
```yaml
components:
  - name: ComponentName
    type: form
    features:
      - validation
      - error handling
      - loading states
```

### Data Model
```yaml
entities:
  - name: EntityName
    fields:
      - id: uuid
      - name: string
      - created_at: timestamp
```

### Infrastructure Requirements
```yaml
infrastructure:
  compute:
    - cpu_cores: 4
    - memory_gb: 8
    - storage_gb: 100
  networking:
    - load_balancer: required
    - cdn: optional
    - ssl_termination: required
  environments:
    development:
      - replicas: 1
      - resources: minimal
    staging:
      - replicas: 2  
      - resources: production-like
    production:
      - replicas: 3
      - resources: optimized
  monitoring:
    - metrics: required
    - logging: centralized
    - alerting: critical_paths
```

### Security Requirements
```yaml
security:
  authentication:
    - method: jwt  # jwt, oauth, saml
    - mfa: required  # required, optional, disabled
    - session_timeout: 30m
  authorization:
    - rbac: required
    - permissions: fine_grained
    - audit_trail: required
  data_protection:
    - encryption_at_rest: required
    - encryption_in_transit: required
    - pii_handling: gdpr_compliant
  compliance:
    - frameworks: ["OWASP", "GDPR"]  # OWASP, GDPR, PCI, SOX, HIPAA
    - penetration_testing: required
    - vulnerability_scanning: automated
  network_security:
    - firewall_rules: restrictive
    - network_segmentation: required
    - intrusion_detection: enabled
```

## Non-Functional Requirements
- Performance: Response time < 100ms for 95th percentile
- Security: OWASP Top 10 compliance
- Scalability: Support 10,000 concurrent users
- Reliability: 99.9% uptime

## Dependencies
<!-- External systems, APIs, or features this depends on -->
- Dependency 1
- Dependency 2

## Risks and Mitigations
<!-- Potential risks and how we'll address them -->
| Risk | Impact | Mitigation |
|------|--------|------------|
| Risk 1 | High | Mitigation strategy |

## Success Metrics
<!-- How we'll measure success -->
- Metric 1: Target value
- Metric 2: Target value

## Out of Scope
<!-- Explicitly state what's NOT included -->
- Feature X
- Integration Y

## Timeline
<!-- Key milestones -->
- Phase 1: [Description] - [Date]
- Phase 2: [Description] - [Date]

## Open Questions
<!-- Unresolved questions needing clarification -->
- [ ] Question 1
- [ ] Question 2
