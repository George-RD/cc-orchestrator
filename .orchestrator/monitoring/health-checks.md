# Health Check Patterns

## Health Check Types

### Liveness Checks
Verify the agent is running and responsive
```yaml
liveness:
  endpoint: /health/live
  interval: 30s
  timeout: 5s
  failure_threshold: 3
  success_threshold: 1
  
  response:
    status: 200
    body:
      alive: true
      agent: agent_name
      version: version_string
```

### Readiness Checks
Verify the agent is ready to accept work
```yaml
readiness:
  endpoint: /health/ready
  interval: 10s
  timeout: 10s
  failure_threshold: 2
  success_threshold: 2
  
  checks:
    - name: dependencies
      critical: true
    - name: resources
      critical: true
    - name: configuration
      critical: true
      
  response:
    status: 200|503
    body:
      ready: boolean
      checks:
        - name: string
          status: pass|fail
          message: string
          duration_ms: number
```

### Deep Health Checks
Comprehensive system verification
```yaml
deep_health:
  endpoint: /health/deep
  interval: 5m
  timeout: 30s
  
  checks:
    - name: code_generation
      test: "Generate simple function"
      expected: "Syntactically valid code"
      
    - name: test_execution
      test: "Run sample test"
      expected: "Test passes"
      
    - name: documentation_generation
      test: "Generate API docs"
      expected: "Valid markdown"
      
  response:
    status: 200|503
    body:
      healthy: boolean
      timestamp: ISO8601
      checks: array
      performance:
        response_time_ms: number
        memory_usage_mb: number
        cpu_usage_percent: number
```

## Agent-Specific Health Checks

### Orchestrator Health
```yaml
orchestrator_health:
  checks:
    - name: agent_discovery
      verify: "Can communicate with all specialist agents"
      
    - name: task_routing
      verify: "Can route tasks to appropriate agents"
      
    - name: coordination_capability
      verify: "Can manage parallel workflows"
      
  thresholds:
    min_available_agents: 3
    max_queue_depth: 100
    max_response_time_ms: 5000
```

### Backend Agent Health
```yaml
backend_health:
  checks:
    - name: code_compilation
      verify: "Generated code compiles"
      
    - name: database_patterns
      verify: "Can generate valid queries"
      
    - name: api_patterns
      verify: "Can create REST endpoints"
      
  capabilities:
    - language: ["javascript", "python", "java"]
    - frameworks: ["express", "django", "spring"]
    - databases: ["postgres", "mysql", "mongodb"]
```

### Frontend Agent Health
```yaml
frontend_health:
  checks:
    - name: component_generation
      verify: "Can create React components"
      
    - name: styling_capability
      verify: "Can apply CSS/Tailwind"
      
    - name: accessibility_check
      verify: "Generates accessible markup"
      
  capabilities:
    - frameworks: ["react", "vue", "angular"]
    - styling: ["css", "tailwind", "styled-components"]
    - build_tools: ["webpack", "vite", "parcel"]
```

### QA Agent Health
```yaml
qa_health:
  checks:
    - name: test_generation
      verify: "Can create test cases"
      
    - name: test_execution
      verify: "Can run tests"
      
    - name: coverage_analysis
      verify: "Can calculate coverage"
      
  capabilities:
    - test_types: ["unit", "integration", "e2e"]
    - frameworks: ["jest", "mocha", "cypress"]
    - analysis: ["coverage", "performance", "security"]
```

### Documentation Agent Health
```yaml
docs_health:
  checks:
    - name: markdown_generation
      verify: "Can create valid markdown"
      
    - name: api_documentation
      verify: "Can document endpoints"
      
    - name: diagram_creation
      verify: "Can create diagrams"
      
  capabilities:
    - formats: ["markdown", "html", "pdf"]
    - diagram_types: ["mermaid", "plantuml", "graphviz"]
    - doc_types: ["api", "user", "developer", "architecture"]
```

## Health Status Aggregation

```yaml
aggregation:
  overall_health:
    calculation: |
      if any(critical_check.status == 'fail'):
        status = 'unhealthy'
      elif any(non_critical_check.status == 'fail'):
        status = 'degraded'
      else:
        status = 'healthy'
        
  scoring:
    healthy: 1.0
    degraded: 0.5
    unhealthy: 0.0
    
  trend_analysis:
    window: 24h
    metrics:
      - availability_percentage
      - performance_score
      - error_rate
```

## Health Check Implementation

### Basic Health Check Handler
```javascript
const healthCheck = async (req, res) => {
  const checks = [];
  const startTime = Date.now();
  
  // Check dependencies
  try {
    await checkDatabaseConnection();
    checks.push({
      name: 'database',
      status: 'pass',
      duration_ms: Date.now() - startTime
    });
  } catch (error) {
    checks.push({
      name: 'database',
      status: 'fail',
      message: error.message,
      duration_ms: Date.now() - startTime
    });
  }
  
  // Check memory usage
  const memUsage = process.memoryUsage();
  const memCheck = memUsage.heapUsed / memUsage.heapTotal;
  checks.push({
    name: 'memory',
    status: memCheck < 0.9 ? 'pass' : 'fail',
    message: `Heap usage: ${(memCheck * 100).toFixed(2)}%`,
    duration_ms: 1
  });
  
  // Overall status
  const hasFailure = checks.some(c => c.status === 'fail');
  const status = hasFailure ? 503 : 200;
  
  res.status(status).json({
    status: hasFailure ? 'unhealthy' : 'healthy',
    timestamp: new Date().toISOString(),
    checks: checks,
    metadata: {
      version: process.env.VERSION,
      uptime: process.uptime(),
      agent: process.env.AGENT_NAME
    }
  });
};
```

### Circuit Breaker Pattern
```javascript
class HealthCircuitBreaker {
  constructor(threshold = 5, timeout = 60000) {
    this.failureCount = 0;
    this.threshold = threshold;
    this.timeout = timeout;
    this.state = 'CLOSED';
    this.nextAttempt = Date.now();
  }
  
  async execute(healthCheck) {
    if (this.state === 'OPEN') {
      if (Date.now() < this.nextAttempt) {
        throw new Error('Circuit breaker is OPEN');
      }
      this.state = 'HALF_OPEN';
    }
    
    try {
      const result = await healthCheck();
      this.onSuccess();
      return result;
    } catch (error) {
      this.onFailure();
      throw error;
    }
  }
  
  onSuccess() {
    this.failureCount = 0;
    if (this.state === 'HALF_OPEN') {
      this.state = 'CLOSED';
    }
  }
  
  onFailure() {
    this.failureCount++;
    if (this.failureCount >= this.threshold) {
      this.state = 'OPEN';
      this.nextAttempt = Date.now() + this.timeout;
    }
  }
}
```

## Monitoring Integration

```yaml
health_metrics:
  export_to:
    - prometheus
    - datadog
    - cloudwatch
    
  metrics:
    - name: health_check_duration
      type: histogram
      labels: [agent, check_type]
      
    - name: health_check_status
      type: gauge
      labels: [agent, check_name]
      values:
        pass: 1
        fail: 0
        
    - name: health_score
      type: gauge
      labels: [agent]
      range: 0.0-1.0
```

## Health-Based Routing

```yaml
routing_strategy:
  health_aware_routing:
    enabled: true
    
    rules:
      - if: agent.health_score < 0.5
        then: route_to_backup_agent
        
      - if: agent.health_score < 0.8
        then: reduce_traffic_weight
        
      - if: agent.consecutive_failures > 3
        then: remove_from_pool
        
  recovery:
    - check_interval: 30s
    - required_successes: 3
    - gradual_traffic_increase: true
```

## Health Dashboard

```yaml
dashboard:
  summary_panel:
    displays:
      - overall_system_health
      - agent_availability_grid
      - recent_failures
      - performance_trends
      
  agent_details:
    displays:
      - health_score_history
      - check_results
      - resource_usage
      - capability_status
      
  alerts_panel:
    displays:
      - active_health_alerts
      - recent_recoveries
      - scheduled_maintenance
```

## Best Practices

1. **Keep health checks lightweight** - Should complete quickly
2. **Test actual functionality** - Not just "return 200"
3. **Include dependency checks** - Verify external services
4. **Use appropriate timeouts** - Prevent hanging checks
5. **Implement graceful degradation** - Partial functionality is better than none
6. **Log health check failures** - For debugging and analysis
7. **Version your health check API** - Support backward compatibility
8. **Cache expensive checks** - Don't repeat costly operations
9. **Implement health check authentication** - Prevent abuse
10. **Monitor the monitors** - Ensure health checks themselves are working
