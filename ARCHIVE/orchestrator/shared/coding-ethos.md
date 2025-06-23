# Shared Coding Ethos

## Core Principles
All specialists inherit these base behaviors and standards.

### TDD Workflow
1. **Write tests first** - No code without tests
2. **Red, Green, Refactor** - Classic TDD cycle
3. **No mocks** - Test real implementations
4. **Comprehensive coverage** - Edge cases matter

### Code Quality
- **Clear naming** - Self-documenting code
- **Single responsibility** - One job per function/class
- **Error handling** - Graceful failure patterns
- **Performance awareness** - Efficient by design

### Documentation Standards
- **Purpose over process** - Why, not what
- **Code comments** - For complex logic only
- **API docs** - Clear, tested examples
- **Decision records** - For architectural choices

### Git Workflow
1. **Commit regularly** during development with clear messages
2. **Final commit** includes confidence level in message

### Handoff Protocol
1. **Update task JSON** with progress and confidence
2. **Document concerns** for medium/low confidence
3. **Commit final state** to worktree branch
4. **Return to orchestrator** for integration

### Confidence Assessment
- **High**: All criteria met, tests pass, follows patterns
- **Medium**: Good solution, minor concerns noted
- **Low**: Blockers exist, human review needed