# Lock Test Files
# Usage: /test-lock <task-id>

Implement test immutability to ensure tests remain stable once written:

## 1. Load Task and Verify Tests

```bash
task_file="/.orchestrator/tasks/active/${1}.json"
if [ ! -f "$task_file" ]; then
    # Check completed tasks too
    task_file="/.orchestrator/tasks/completed/${1}.json"
fi

# Extract test files from task artifacts
test_files=$(jq -r '.artifacts.test_files[]' "$task_file")
```

## 2. Pre-Lock Validation

Before locking, ensure:
```yaml
validation_checklist:
  tests_exist:
    - At least one test file in artifacts
    - Test files actually exist on disk
    
  tests_passing:
    - Run test suite
    - All tests must pass
    - No skipped tests
    
  tests_complete:
    - Coverage meets minimum (80%)
    - All acceptance criteria tested
    - Edge cases covered
```

## 3. Apply Lock Protection

### 3.1 Add Lock Header to Test Files
For each test file:
```javascript
// ====================
// 🔒 LOCKED TEST FILE
// ====================
// This test file is locked and should not be modified
// without orchestrator approval and documentation.
// 
// Locked by: ${agent}
// Locked at: ${timestamp}
// Task ID: ${task_id}
// 
// To request changes:
// 1. Use /test-modify-request command
// 2. Document the reason for change
// 3. Await orchestrator approval
// ====================

[original test content]
```

### 3.2 Create Lock Marker
```bash
# Create .test-lock file in test directory
echo '{
  "locked_at": "'$(date -u +"%Y-%m-%dT%H:%M:%SZ")'",
  "locked_by": "'${agent}'",
  "task_id": "'${1}'",
  "test_files": ['${test_files}'],
  "lock_version": "1.0"
}' > $(dirname ${test_file})/.test-lock
```

### 3.3 Update Task JSON
```json
{
  "test_protection": {
    "tests_locked": true,
    "locked_at": "2025-01-15T10:00:00Z",
    "locked_by": "qa",
    "lock_reason": "Tests validated and passing",
    "modification_requests": []
  }
}
```

## 4. Git Protection

### 4.1 Create Protected Branch Rule
```bash
# Add to .github/CODEOWNERS if exists
**/tests/*    @orchestrator-approval-required
**/*.test.*   @orchestrator-approval-required
```

### 4.2 Commit Lock State
```bash
git add ${test_files} .test-lock
git commit -m "test: Lock test files for ${1}

- Tests are now immutable
- Changes require orchestrator approval
- All tests passing with ${coverage}% coverage"
```

## 5. Lock Registry

Track all locked tests:
```json
// In /.orchestrator/tasks/test-locks.json
{
  "locked_tests": {
    "${1}": {
      "files": ["test1.js", "test2.js"],
      "locked_at": "timestamp",
      "coverage": "92%",
      "total_tests": 24
    }
  },
  "total_locked_files": 45,
  "last_updated": "timestamp"
}
```

## 6. Modification Request Process

If changes needed later:

### 6.1 Request Command
`/test-modify-request <task-id> <reason>`

Creates modification request:
```json
{
  "request_id": "mod-req-001",
  "task_id": "${1}",
  "requested_by": "backend",
  "requested_at": "timestamp",
  "reason": "New edge case discovered in production",
  "changes_needed": [
    "Add test for null input handling",
    "Update assertion for new validation"
  ],
  "impact": "minor",
  "risk": "low"
}
```

### 6.2 Approval Workflow
```yaml
approval_criteria:
  auto_approve_if:
    - Adding new tests only (no modifications)
    - Fixing typos in comments
    - Updating test descriptions
    
  requires_review_if:
    - Changing assertions
    - Modifying test logic
    - Removing tests
    - Changing test data
```

## 7. Output Confirmation

```
🔒 Tests Locked Successfully
════════════════════════════════════════════

Task: ${task_id}
Test Files Locked: ${count}
Coverage at Lock: ${coverage}%
Total Tests: ${test_count}

Files Protected:
- ✓ tests/api/auth.test.js
- ✓ tests/unit/validation.test.js
- ✓ tests/integration/user-flow.test.js

Protection Applied:
- Lock headers added to files
- .test-lock marker created
- Task JSON updated
- Git commit created

These tests are now immutable. Changes require:
1. Modification request: /test-modify-request
2. Orchestrator approval
3. Documented rationale

Lock ID: ${lock_id}
```

## 8. Unlock Process (Emergency Only)

For critical fixes:
```bash
/test-unlock ${task_id} --emergency --reason "Critical bug fix"

# Requires:
# - Orchestrator role
# - Documented reason
# - Immediate re-lock after fix
```

## 9. Best Practices

### When to Lock Tests:
- After task completion
- When tests are stable
- Before major refactoring
- At sprint boundaries

### What to Lock:
- Unit tests
- Integration tests  
- E2E test scenarios
- Test fixtures/data

### What NOT to Lock:
- Test utilities/helpers
- Mock implementations
- Test configuration
- Development fixtures

---
*Tests locked for stability. Modifications require approval.*
