# Inter-Agent Communication Protocol

## Message Format
All inter-agent communication follows this JSON structure:

```json
{
  "message_id": "uuid",
  "timestamp": "ISO8601",
  "from_agent": "agent_name",
  "to_agent": "agent_name",
  "message_type": "request|response|notification|error",
  "correlation_id": "uuid (for request/response pairs)",
  "priority": "low|normal|high|critical",
  "payload": {
    // Message-specific content
  },
  "metadata": {
    "confidence": 0.0-1.0,
    "processing_time_ms": 123,
    "version": "1.0"
  }
}
```

## Message Types

### Request
Used when an agent needs another agent to perform work:
```json
{
  "message_type": "request",
  "payload": {
    "action": "string",
    "parameters": {},
    "context": {},
    "constraints": {},
    "deadline": "ISO8601 (optional)"
  }
}
```

### Response
Reply to a request:
```json
{
  "message_type": "response",
  "correlation_id": "original_request_id",
  "payload": {
    "status": "success|failure|partial",
    "result": {},
    "errors": [],
    "warnings": []
  }
}
```

### Notification
Broadcast information without expecting response:
```json
{
  "message_type": "notification",
  "payload": {
    "event": "string",
    "data": {},
    "severity": "info|warning|error"
  }
}
```

### Error
Communicate failures:
```json
{
  "message_type": "error",
  "payload": {
    "error_code": "string",
    "error_message": "string",
    "details": {},
    "recovery_suggestions": []
  }
}
```

## Standard Actions

### Code Review Request
```json
{
  "action": "review_code",
  "parameters": {
    "files": ["path1", "path2"],
    "review_type": "security|performance|style|functionality",
    "urgency": "low|normal|high"
  }
}
```

### Test Execution Request
```json
{
  "action": "run_tests",
  "parameters": {
    "test_suite": "unit|integration|e2e|all",
    "files": ["specific files or all"],
    "coverage_required": true
  }
}
```

### Documentation Update Request
```json
{
  "action": "update_documentation",
  "parameters": {
    "change_type": "api|feature|bugfix",
    "affected_components": [],
    "version": "string"
  }
}
```

## Communication Patterns

### Synchronous Request-Response
1. Agent A sends request with unique message_id
2. Agent B processes and responds with correlation_id
3. Agent A handles response

### Asynchronous Task with Callback
1. Agent A sends request with callback_url
2. Agent B acknowledges receipt immediately
3. Agent B processes asynchronously
4. Agent B sends notification when complete

### Broadcast Pattern
1. Agent sends notification to all agents
2. Interested agents process independently
3. No response expected

## Best Practices

1. **Always include confidence scores** for decision transparency
2. **Use correlation IDs** for request/response tracking
3. **Set realistic deadlines** for time-sensitive requests
4. **Include context** for better decision making
5. **Handle errors gracefully** with recovery suggestions
6. **Version your messages** for backward compatibility
7. **Log all communications** for debugging and audit

## Error Handling

Standard error codes:
- `AGENT_UNAVAILABLE`: Target agent not responding
- `INVALID_REQUEST`: Malformed or missing parameters
- `TIMEOUT`: Request deadline exceeded
- `CAPACITY_EXCEEDED`: Agent overloaded
- `UNAUTHORIZED`: Insufficient permissions
- `INTERNAL_ERROR`: Unexpected agent failure

## Performance Guidelines

- Keep payload size < 1MB
- Set appropriate timeouts (default: 30s)
- Use compression for large payloads
- Batch related requests when possible
- Implement circuit breakers for failing agents
