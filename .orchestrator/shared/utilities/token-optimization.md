# Token Optimization Strategies for Claude Code

## Overview
Claude Code operates on a token-based pricing model. These strategies help minimize token usage while maintaining effectiveness.

## Core Optimization Strategies

### 1. Batch Operations
Group related operations to reduce overhead:

```javascript
// ❌ Inefficient: Multiple separate reads
const file1 = await readFile('src/component1.js');
const file2 = await readFile('src/component2.js');
const file3 = await readFile('src/component3.js');

// ✅ Efficient: Batch read
const files = await readMultipleFiles([
  'src/component1.js',
  'src/component2.js', 
  'src/component3.js'
]);
```

### 2. Targeted File Access
Be specific about what you need:

```javascript
// ❌ Inefficient: Search entire codebase
const files = await searchFiles('.', '*.js');

// ✅ Efficient: Target specific directories
const files = await searchFiles('src/components', '*.js');

// ✅ Even better: Direct file access when paths are known
const file = await readFile('src/components/UserAuth.js');
```

### 3. Incremental Updates
Modify only what's necessary:

```javascript
// ❌ Inefficient: Rewrite entire file
const content = await readFile('config.json');
const config = JSON.parse(content);
config.version = '2.0.0';
await writeFile('config.json', JSON.stringify(config, null, 2));

// ✅ Efficient: Targeted edit
await editFile('config.json', {
  oldText: '"version": "1.0.0"',
  newText: '"version": "2.0.0"'
});
```

## Model Selection Strategy

### Decision Framework
```yaml
use_sonnet_4_for:
  - routine_code_generation
  - simple_refactoring  
  - test_writing
  - documentation_updates
  - file_operations
  - basic_debugging

use_opus_4_for:
  - architecture_decisions
  - complex_problem_solving
  - security_analysis
  - performance_optimization
  - api_design
  - code_review_synthesis
```

### Model Switching Example
```javascript
// In orchestrator logic
const selectModel = (task) => {
  const opusRequiredTasks = [
    'architecture_decision',
    'security_review',
    'complex_algorithm',
    'api_redesign'
  ];
  
  return opusRequiredTasks.includes(task.type) ? 'opus-4' : 'sonnet-4';
};
```

## Permission Request Optimization

### Batch Permission Requests
```yaml
# Instead of requesting permissions for each file:
# ❌ Read src/index.js? [y/n]
# ❌ Read src/utils.js? [y/n] 
# ❌ Read src/config.js? [y/n]

# Request permissions for entire workflow:
# ✅ This task requires reading and modifying files in src/
#    - Read: src/**/*.js
#    - Write: src/**/*.js
#    - Execute: npm test
#    Approve all? [y/n]
```

### Permission Patterns
```javascript
// Define permission sets for common workflows
const permissionSets = {
  featureDevelopment: {
    read: ['src/**', 'tests/**', 'package.json'],
    write: ['src/**', 'tests/**'],
    execute: ['npm test', 'npm run lint']
  },
  
  bugFix: {
    read: ['src/**', 'tests/**', 'logs/**'],
    write: ['src/**', 'tests/**'],
    execute: ['npm test']
  },
  
  documentation: {
    read: ['**/*.md', 'src/**'],
    write: ['**/*.md', 'docs/**']
  }
};
```

## Context Management

### Selective Context Loading
```javascript
// ❌ Inefficient: Load entire project context
const allFiles = await searchFiles('.', '*');
const context = await Promise.all(
  allFiles.map(f => readFile(f))
);

// ✅ Efficient: Load only relevant context
const getRelevantContext = async (feature) => {
  const relevantPaths = {
    'authentication': ['src/auth/**', 'src/middleware/auth.js'],
    'api': ['src/routes/**', 'src/controllers/**'],
    'frontend': ['src/components/**', 'src/pages/**']
  };
  
  const paths = relevantPaths[feature] || [];
  return await readMultipleFiles(paths);
};
```

### Context Caching
```javascript
// Cache frequently accessed content
const contextCache = new Map();

const getCachedContent = async (path) => {
  if (contextCache.has(path)) {
    return contextCache.get(path);
  }
  
  const content = await readFile(path);
  contextCache.set(path, content);
  return content;
};

// Clear cache when files are modified
const updateFile = async (path, content) => {
  await writeFile(path, content);
  contextCache.delete(path);
};
```

## Search Optimization

### Progressive Search Refinement
```javascript
// Start broad, then narrow
const findRelevantFiles = async (topic) => {
  // 1. Quick directory scan
  const dirs = await listDirectory('src');
  
  // 2. Filter likely directories
  const relevantDirs = dirs.filter(d => 
    d.includes(topic) || relatedTerms[topic].some(t => d.includes(t))
  );
  
  // 3. Search only relevant directories
  const files = [];
  for (const dir of relevantDirs) {
    files.push(...await searchFiles(dir, `*${topic}*`));
  }
  
  return files;
};
```

### Search Result Limits
```javascript
// Limit search results to avoid token explosion
const searchWithLimit = async (pattern, maxResults = 20) => {
  const results = await searchFiles('.', pattern);
  
  if (results.length > maxResults) {
    console.log(`Found ${results.length} files, showing first ${maxResults}`);
    return results.slice(0, maxResults);
  }
  
  return results;
};
```

## Communication Efficiency

### Structured Responses
```javascript
// Use concise, structured formats
const reportResult = (result) => {
  return {
    status: result.success ? 'success' : 'failed',
    summary: result.summary, // Brief description
    details: result.critical ? result.details : null, // Include details only when critical
    next: result.nextSteps // Clear next actions
  };
};
```

### Avoid Redundancy
```javascript
// ❌ Inefficient: Repeating information
"I've analyzed the authentication system. The authentication system has several issues. 
Let me explain each issue with the authentication system..."

// ✅ Efficient: Concise communication
"Authentication analysis revealed 3 issues:
1. Weak password validation
2. Missing rate limiting  
3. No session timeout
[Details for each...]"
```

## Task Decomposition Optimization

### Granular Task Sizing
```yaml
# Balance task size for optimal token usage
task_sizing:
  too_small:  # Causes excessive handoffs
    - "Add a comment"
    - "Fix indentation"
    
  optimal:    # Minimal handoffs, clear scope
    - "Implement user authentication endpoint"
    - "Create responsive navigation component"
    - "Write integration tests for API"
    
  too_large:  # Requires multiple model calls
    - "Build entire application"
    - "Refactor all code"
```

### Parallel vs Sequential Processing
```javascript
// Identify independent tasks for parallel processing
const analyzeTasks = (tasks) => {
  const dependencies = buildDependencyGraph(tasks);
  
  return {
    parallel: tasks.filter(t => dependencies[t.id].length === 0),
    sequential: tasks.filter(t => dependencies[t.id].length > 0)
  };
};

// Execute parallel tasks in one batch
const executeParallelTasks = async (tasks) => {
  return await Promise.all(
    tasks.map(task => executeTask(task))
  );
};
```

## Monitoring Token Usage

### Usage Tracking
```javascript
// Track token usage patterns
const tokenTracker = {
  operations: [],
  
  record: function(operation, estimatedTokens) {
    this.operations.push({
      timestamp: Date.now(),
      operation,
      tokens: estimatedTokens
    });
  },
  
  getStats: function() {
    const total = this.operations.reduce((sum, op) => sum + op.tokens, 0);
    const byOperation = {};
    
    this.operations.forEach(op => {
      byOperation[op.operation] = (byOperation[op.operation] || 0) + op.tokens;
    });
    
    return { total, byOperation };
  }
};
```

### Optimization Feedback
```javascript
// Identify optimization opportunities
const analyzeUsage = (tracker) => {
  const stats = tracker.getStats();
  const suggestions = [];
  
  // Check for repeated operations
  const repeated = Object.entries(stats.byOperation)
    .filter(([op, count]) => count > 3);
    
  if (repeated.length > 0) {
    suggestions.push({
      issue: 'Repeated operations detected',
      solution: 'Consider batching or caching',
      operations: repeated
    });
  }
  
  return suggestions;
};
```

## Best Practices Summary

1. **Batch Everything**: Group related operations
2. **Be Specific**: Use exact paths over searches
3. **Cache Wisely**: Store frequently accessed content
4. **Model Appropriately**: Use Sonnet for routine, Opus for complex
5. **Limit Scope**: Process only what's necessary
6. **Structure Communication**: Use concise, clear formats
7. **Monitor Usage**: Track and optimize patterns
8. **Plan Workflows**: Design for minimal handoffs
9. **Parallelize**: Run independent tasks simultaneously
10. **Fail Fast**: Validate early to avoid wasted tokens
