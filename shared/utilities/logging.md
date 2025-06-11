# Standardized Logging Patterns

## Log Format
All agents use structured logging with consistent format:

```json
{
  "timestamp": "ISO8601",
  "level": "DEBUG|INFO|WARN|ERROR|FATAL",
  "agent": "agent_name",
  "correlation_id": "uuid",
  "message": "Human readable message",
  "context": {
    "task_id": "uuid",
    "user_id": "uuid",
    "action": "string",
    "duration_ms": 123
  },
  "metadata": {
    "file": "filename",
    "line": 123,
    "function": "function_name"
  },
  "error": {
    "type": "ErrorType",
    "message": "Error details",
    "stack": "Stack trace"
  }
}
```

## Log Levels

### DEBUG
Detailed information for diagnosing problems:
```javascript
log.debug("Analyzing code structure", {
  files_count: 42,
  complexity: "medium",
  patterns_found: ["mvc", "singleton"]
});
```

### INFO
General informational messages:
```javascript
log.info("Task completed successfully", {
  task_id: "t-123",
  duration_ms: 1234,
  result: "success"
});
```

### WARN
Warning conditions that don't prevent operation:
```javascript
log.warn("Deprecated API usage detected", {
  api: "oldMethod",
  replacement: "newMethod",
  location: "user-service.js:45"
});
```

### ERROR
Error conditions that require attention:
```javascript
log.error("Failed to process request", {
  error_type: "ValidationError",
  details: "Invalid email format",
  user_input: "not-an-email"
});
```

### FATAL
Critical errors requiring immediate action:
```javascript
log.fatal("Database connection lost", {
  connection_string: "postgres://...",
  retry_count: 5,
  action: "shutting_down"
});
```

## Contextual Logging

### Task Context
Always include task context:
```javascript
const withTaskContext = (taskId, action) => {
  return {
    task_id: taskId,
    action: action,
    timestamp: new Date().toISOString()
  };
};

log.info("Starting code review", withTaskContext(taskId, "review_start"));
```

### Performance Logging
Track operation duration:
```javascript
const startTime = Date.now();
// ... operation ...
log.info("Operation completed", {
  ...context,
  duration_ms: Date.now() - startTime,
  performance: {
    cpu_usage: process.cpuUsage(),
    memory: process.memoryUsage()
  }
});
```

### Error Context
Include comprehensive error information:
```javascript
try {
  // ... operation ...
} catch (error) {
  log.error("Operation failed", {
    error_type: error.constructor.name,
    error_message: error.message,
    error_stack: error.stack,
    error_code: error.code,
    recovery_attempted: true,
    recovery_successful: false
  });
}
```

## Log Aggregation Patterns

### Correlation IDs
Track related operations:
```javascript
const correlationId = generateUUID();

// In orchestrator
log.info("Delegating to backend agent", {
  correlation_id: correlationId,
  agent: "orchestrator",
  target: "backend"
});

// In backend agent
log.info("Received task", {
  correlation_id: correlationId,
  agent: "backend",
  source: "orchestrator"
});
```

### Transaction Logging
Log complete transaction flow:
```javascript
const transaction = {
  id: generateUUID(),
  start: Date.now(),
  steps: []
};

// Log each step
transaction.steps.push({
  name: "validation",
  result: "success",
  duration: 45
});

// Log transaction summary
log.info("Transaction completed", {
  transaction_id: transaction.id,
  total_duration: Date.now() - transaction.start,
  steps_count: transaction.steps.length,
  success: true
});
```

## Best Practices

### 1. Structured Over Strings
```javascript
// ❌ Bad
log.info(`User ${userId} performed ${action} on ${resource}`);

// ✅ Good
log.info("User action performed", {
  user_id: userId,
  action: action,
  resource: resource
});
```

### 2. Consistent Field Names
```javascript
// Use consistent names across all agents:
// user_id (not userId, user, uid)
// task_id (not taskId, task, tid)
// error_message (not error, message, err_msg)
```

### 3. Avoid Sensitive Data
```javascript
// ❌ Bad
log.info("User login", {
  email: user.email,
  password: user.password  // Never log passwords!
});

// ✅ Good
log.info("User login", {
  user_id: user.id,
  email_domain: user.email.split('@')[1],
  auth_method: "password"
});
```

### 4. Actionable Messages
```javascript
// ❌ Bad
log.error("Something went wrong");

// ✅ Good
log.error("Database query timeout", {
  query_type: "user_fetch",
  timeout_ms: 5000,
  suggestion: "Check database connection and query performance"
});
```

## Log Analysis Queries

### Common Patterns
```sql
-- Find slow operations
SELECT * FROM logs 
WHERE duration_ms > 1000 
AND level = 'INFO'
ORDER BY duration_ms DESC;

-- Error frequency by type
SELECT error_type, COUNT(*) as count
FROM logs
WHERE level = 'ERROR'
GROUP BY error_type
ORDER BY count DESC;

-- Agent performance
SELECT agent, 
       AVG(duration_ms) as avg_duration,
       COUNT(*) as operations
FROM logs
WHERE action = 'task_complete'
GROUP BY agent;
```

## Integration Examples

### With Monitoring Systems
```javascript
// Prometheus metrics from logs
const logWithMetrics = (level, message, context) => {
  // Standard log
  log[level](message, context);
  
  // Update metrics
  if (context.duration_ms) {
    histogram.observe(context.duration_ms);
  }
  if (level === 'ERROR') {
    errorCounter.inc({ error_type: context.error_type });
  }
};
```

### With Alerting
```javascript
// Trigger alerts for critical conditions
if (level === 'FATAL' || 
    (level === 'ERROR' && context.error_count > 5)) {
  alertingService.send({
    severity: 'high',
    message: message,
    context: context
  });
}
```
