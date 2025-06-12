# Performance Optimization Command
# Usage: /optimize [component/area]

Comprehensive performance optimization workflow for ${1}:

## 1. Performance Baseline
- Measure current performance metrics
- Identify bottlenecks using profiling tools
- Document baseline numbers

## 2. Analysis Areas

### Frontend Performance
- [ ] Bundle size analysis
- [ ] Lighthouse score assessment
- [ ] Core Web Vitals (LCP, FID, CLS)
- [ ] Network waterfall analysis
- [ ] Rendering performance

### Backend Performance  
- [ ] API response times
- [ ] Database query performance
- [ ] Memory usage patterns
- [ ] CPU utilization
- [ ] Concurrent request handling

### Database Performance
- [ ] Query execution plans
- [ ] Index usage analysis
- [ ] Connection pool optimization
- [ ] Cache hit rates
- [ ] Lock contention

## 3. Optimization Strategies

Based on findings, apply relevant optimizations:

### Code Level
- Algorithm optimization
- Lazy loading implementation
- Code splitting
- Tree shaking
- Memoization

### Database Level
- Query optimization
- Index creation/optimization
- Denormalization where appropriate
- Connection pooling
- Query result caching

### Infrastructure Level
- CDN implementation
- Caching strategy (Redis/Memcached)
- Load balancing
- Auto-scaling configuration

## 4. Implementation Plan
1. Prioritize optimizations by impact/effort ratio
2. Implement one optimization at a time
3. Measure impact after each change
4. Document configuration changes

## 5. Validation
- Re-run performance tests
- Compare against baseline
- Ensure no functionality regression
- Monitor for sustained improvement

## 6. Documentation
- Update performance benchmarks
- Document optimization techniques used
- Create runbook for future optimization

Reference monitoring/benchmarks.md for detailed metrics and targets.
