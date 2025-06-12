# Debug Command
# Usage: /debug <issue-description>

Systematic debugging approach for ${1}:

## 1. Issue Analysis
- Understand the reported problem
- Identify affected components
- Determine scope and severity

## 2. Reproduction
- Create minimal reproduction steps
- Verify the issue exists
- Document the expected vs actual behavior

## 3. Investigation Strategy
- Check recent changes that might have introduced the issue
- Review related error logs and monitoring data
- Examine the code path involved

## 4. Root Cause Analysis
- Use backward analysis from specialists/qa.md
- Trace through the execution flow
- Identify the exact point of failure

## 5. Solution Development
- Develop fix following TDD approach
- Write test to verify the bug
- Implement minimal fix
- Ensure no regression

## 6. Validation
- Verify fix resolves the issue
- Run related test suites
- Check for side effects

## 7. Documentation
- Update relevant documentation
- Add to troubleshooting guide if applicable
- Create ADR if architectural change needed

Use systematic approach and maintain clear communication throughout the debugging process.
