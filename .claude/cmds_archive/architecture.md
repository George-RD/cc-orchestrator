# Architecture Decision Command
# Usage: /architecture <decision-topic>

Guide for making and documenting architectural decisions about ${1}:

## 1. Trigger Ultra-Think Mode
For complex architectural decisions, I'll engage deep analysis mode to consider all implications.

"Think harder about the architectural implications of ${1}"

## 2. Decision Framework

### Context Gathering
- What problem are we trying to solve?
- What are the current pain points?
- What constraints do we have?
- What are the success criteria?

### Requirements Analysis
- Functional requirements
- Non-functional requirements (performance, security, scalability)
- Business constraints
- Technical constraints

### Solution Exploration
- Identify at least 3 viable options
- Research industry best practices
- Consider team expertise
- Evaluate against our specific context

### Trade-off Analysis
For each option, analyze:
- **Pros**: Benefits and advantages
- **Cons**: Drawbacks and risks
- **Complexity**: Implementation and maintenance effort
- **Cost**: Development and operational costs
- **Scalability**: Growth potential
- **Security**: Security implications
- **Performance**: Performance characteristics

### Decision Matrix
Create a weighted scoring matrix:
- List all evaluation criteria
- Assign weights based on importance
- Score each option (1-5)
- Calculate weighted scores

## 3. Validation Process
- [ ] Review with technical stakeholders
- [ ] Assess security implications
- [ ] Validate against architectural principles
- [ ] Check compliance requirements
- [ ] Consider operational impact

## 4. Implementation Planning
- Define implementation phases
- Identify required resources
- Create migration strategy (if applicable)
- Plan rollback procedures
- Set success metrics

## 5. Documentation
Create an Architecture Decision Record (ADR):
- Clear problem statement
- Detailed context
- Decision rationale
- Expected consequences
- Implementation plan

## 6. Communication
- Present to relevant stakeholders
- Address concerns and questions
- Get necessary approvals
- Communicate to broader team

Remember: Good architecture decisions are reversible or have clear migration paths.
