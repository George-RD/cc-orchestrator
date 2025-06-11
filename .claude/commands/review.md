# Code Review Command
# Usage: /review [focus-area]

Perform a comprehensive code review with the following focus areas:

## Review Checklist

### Code Quality (always checked)
- [ ] Follows project coding standards
- [ ] Clear and meaningful variable/function names  
- [ ] Appropriate comments for complex logic
- [ ] No code duplication (DRY principle)
- [ ] Proper error handling

### Security Review (when focus=security or always for auth/payment code)
- [ ] Input validation implemented
- [ ] No SQL injection vulnerabilities
- [ ] XSS prevention measures
- [ ] Proper authentication/authorization
- [ ] Sensitive data properly handled
- [ ] Dependencies scanned for vulnerabilities

### Performance Review (when focus=performance or for data-heavy operations)
- [ ] Database queries optimized with proper indexes
- [ ] Caching implemented where appropriate
- [ ] No N+1 query problems
- [ ] Pagination for large datasets
- [ ] Async operations for I/O bound tasks

### Testing Review (always checked)
- [ ] Unit tests present and meaningful
- [ ] Edge cases covered
- [ ] Integration tests for external dependencies
- [ ] Test coverage meets project standards (>80%)

### Documentation Review
- [ ] API endpoints documented
- [ ] Complex algorithms explained
- [ ] README updated if needed
- [ ] Type definitions complete (TypeScript)

Output format:
1. Summary of findings
2. Critical issues that must be fixed
3. Suggestions for improvement
4. Positive aspects to reinforce good practices

Use the consensus-based approach from .orchestrator/specialists/qa.md for thorough analysis.
