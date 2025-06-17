# Security Testing Patterns

## Overview
This document defines standardized security testing patterns used by QA and other specialists.

## OWASP Top 10 Test Patterns

### A01: Broken Access Control Tests
```javascript
describe('Access Control Security Tests', () => {
  it('should enforce proper access controls', async () => {
    const userToken = await loginAs('regular-user');
    const response = await request(app)
      .get('/api/admin/users')
      .set('Authorization', `Bearer ${userToken}`);
    
    expect(response.status).toBe(403);
    expect(response.body.error).toContain('Insufficient permissions');
  });
  
  it('should prevent horizontal privilege escalation', async () => {
    const user1Token = await loginAs('user1');
    const response = await request(app)
      .get('/api/users/user2/profile')
      .set('Authorization', `Bearer ${user1Token}`);
    
    expect(response.status).toBe(403);
  });
  
  it('should prevent vertical privilege escalation', async () => {
    const userToken = await loginAs('regular-user');
    const response = await request(app)
      .post('/api/admin/promote-user')
      .set('Authorization', `Bearer ${userToken}`)
      .send({ userId: 'user123', role: 'admin' });
    
    expect(response.status).toBe(403);
  });
});
```

### A02: Cryptographic Failures Tests
```javascript
describe('Cryptographic Security Tests', () => {
  it('should not expose sensitive data in responses', async () => {
    const response = await request(app)
      .get('/api/users/123');
    
    expect(response.body).not.toHaveProperty('password');
    expect(response.body).not.toHaveProperty('ssn');
    expect(response.body).not.toHaveProperty('creditCard');
    expect(response.body).not.toHaveProperty('apiKey');
  });
  
  it('should use HTTPS for all sensitive endpoints', async () => {
    const response = await request(app)
      .get('/api/auth/login');
    
    expect(response.headers['strict-transport-security']).toBeDefined();
  });
  
  it('should properly hash passwords', async () => {
    const password = 'testPassword123!';
    const hash = await hashPassword(password);
    
    expect(hash).not.toBe(password);
    expect(hash.length).toBeGreaterThan(50);
    expect(await verifyPassword(password, hash)).toBe(true);
  });
});
```

### A03: Injection Tests
```javascript
describe('Injection Security Tests', () => {
  it('should prevent SQL injection', async () => {
    const maliciousInputs = [
      "admin'--",
      "1' OR '1'='1",
      "'; DROP TABLE users; --",
      "admin'; DELETE FROM users WHERE '1'='1"
    ];
    
    for (const input of maliciousInputs) {
      const response = await request(app)
        .post('/api/auth/login')
        .send({ email: input, password: 'password' });
      
      expect(response.status).toBe(400);
      
      // Verify database integrity
      const userCount = await db.query('SELECT COUNT(*) as count FROM users');
      expect(userCount.rows[0].count).toBeGreaterThan(0);
    }
  });
  
  it('should prevent NoSQL injection', async () => {
    const response = await request(app)
      .post('/api/auth/login')
      .send({ 
        email: { $ne: null }, 
        password: { $ne: null } 
      });
    
    expect(response.status).toBe(400);
  });
  
  it('should prevent command injection', async () => {
    const response = await request(app)
      .post('/api/files/process')
      .send({ filename: 'test.txt; rm -rf /' });
    
    expect(response.status).toBe(400);
  });
});
```

### A04: Insecure Design Tests
```javascript
describe('Design Security Tests', () => {
  it('should implement proper rate limiting', async () => {
    const requests = Array(100).fill().map(() => 
      request(app).post('/api/auth/login').send({ 
        email: 'test@example.com', 
        password: 'wrong' 
      })
    );
    
    const responses = await Promise.all(requests);
    const rateLimitedResponses = responses.filter(r => r.status === 429);
    
    expect(rateLimitedResponses.length).toBeGreaterThan(0);
  });
  
  it('should implement account lockout after failed attempts', async () => {
    // Attempt multiple failed logins
    for (let i = 0; i < 6; i++) {
      await request(app)
        .post('/api/auth/login')
        .send({ email: 'test@example.com', password: 'wrong' });
    }
    
    // Next attempt should be blocked
    const response = await request(app)
      .post('/api/auth/login')
      .send({ email: 'test@example.com', password: 'correct' });
    
    expect(response.status).toBe(423); // Locked
  });
});
```

### A05: Security Misconfiguration Tests
```javascript
describe('Configuration Security Tests', () => {
  it('should set secure headers', async () => {
    const response = await request(app).get('/');
    
    expect(response.headers['x-content-type-options']).toBe('nosniff');
    expect(response.headers['x-frame-options']).toBe('DENY');
    expect(response.headers['x-xss-protection']).toBe('1; mode=block');
  });
  
  it('should not expose server information', async () => {
    const response = await request(app).get('/');
    
    expect(response.headers.server).toBeUndefined();
    expect(response.headers['x-powered-by']).toBeUndefined();
  });
  
  it('should disable directory listing', async () => {
    const response = await request(app).get('/static/');
    
    expect(response.status).not.toBe(200);
    expect(response.text).not.toContain('Index of');
  });
});
```

## Authentication Security Tests

### Session Management
```javascript
describe('Session Security Tests', () => {
  it('should invalidate sessions on logout', async () => {
    const loginResponse = await request(app)
      .post('/api/auth/login')
      .send({ email: 'test@example.com', password: 'password' });
    
    const token = loginResponse.body.token;
    
    // Logout
    await request(app)
      .post('/api/auth/logout')
      .set('Authorization', `Bearer ${token}`);
    
    // Try to use old token
    const response = await request(app)
      .get('/api/profile')
      .set('Authorization', `Bearer ${token}`);
    
    expect(response.status).toBe(401);
  });
  
  it('should expire sessions after timeout', async () => {
    const loginResponse = await request(app)
      .post('/api/auth/login')
      .send({ email: 'test@example.com', password: 'password' });
    
    const token = loginResponse.body.token;
    
    // Mock time passage
    jest.advanceTimersByTime(16 * 60 * 1000); // 16 minutes
    
    const response = await request(app)
      .get('/api/profile')
      .set('Authorization', `Bearer ${token}`);
    
    expect(response.status).toBe(401);
  });
});
```

### Password Security Tests
```javascript
describe('Password Security Tests', () => {
  it('should enforce password complexity', async () => {
    const weakPasswords = [
      'password',
      '123456',
      'abc123',
      'password123'
    ];
    
    for (const password of weakPasswords) {
      const response = await request(app)
        .post('/api/auth/register')
        .send({ 
          email: 'test@example.com', 
          password: password 
        });
      
      expect(response.status).toBe(400);
      expect(response.body.error).toContain('password');
    }
  });
  
  it('should prevent password reuse', async () => {
    // Register with initial password
    await request(app)
      .post('/api/auth/register')
      .send({ 
        email: 'test@example.com', 
        password: 'OldPassword123!' 
      });
    
    const loginResponse = await request(app)
      .post('/api/auth/login')
      .send({ 
        email: 'test@example.com', 
        password: 'OldPassword123!' 
      });
    
    // Try to change back to same password
    const response = await request(app)
      .post('/api/auth/change-password')
      .set('Authorization', `Bearer ${loginResponse.body.token}`)
      .send({ 
        currentPassword: 'OldPassword123!',
        newPassword: 'OldPassword123!' 
      });
    
    expect(response.status).toBe(400);
    expect(response.body.error).toContain('reuse');
  });
});
```

## Input Validation Security Tests

### Cross-Site Scripting (XSS) Tests
```javascript
describe('XSS Security Tests', () => {
  it('should prevent stored XSS', async () => {
    const xssPayloads = [
      '<script>alert("xss")</script>',
      '<img src="x" onerror="alert(1)">',
      'javascript:alert(1)',
      '<svg onload="alert(1)">'
    ];
    
    for (const payload of xssPayloads) {
      const response = await request(app)
        .post('/api/comments')
        .send({ content: payload });
      
      expect(response.status).toBe(400);
    }
  });
  
  it('should sanitize output', async () => {
    const response = await request(app)
      .get('/api/user-content/123');
    
    expect(response.body.content).not.toContain('<script>');
    expect(response.body.content).not.toContain('javascript:');
  });
});
```

## API Security Tests

### Authorization Tests
```javascript
describe('API Authorization Tests', () => {
  it('should require authentication for protected endpoints', async () => {
    const protectedEndpoints = [
      { method: 'GET', path: '/api/profile' },
      { method: 'POST', path: '/api/posts' },
      { method: 'PUT', path: '/api/users/123' },
      { method: 'DELETE', path: '/api/posts/123' }
    ];
    
    for (const endpoint of protectedEndpoints) {
      const response = await request(app)[endpoint.method.toLowerCase()](endpoint.path);
      expect(response.status).toBe(401);
    }
  });
  
  it('should validate JWT tokens properly', async () => {
    const invalidTokens = [
      'invalid.token.here',
      'Bearer invalid',
      'expired.jwt.token',
      ''
    ];
    
    for (const token of invalidTokens) {
      const response = await request(app)
        .get('/api/profile')
        .set('Authorization', `Bearer ${token}`);
      
      expect(response.status).toBe(401);
    }
  });
});
```

## Performance Security Tests

### DoS Protection Tests
```javascript
describe('DoS Protection Tests', () => {
  it('should handle large payloads gracefully', async () => {
    const largePayload = 'x'.repeat(10 * 1024 * 1024); // 10MB
    
    const response = await request(app)
      .post('/api/data')
      .send({ data: largePayload });
    
    expect([413, 400]).toContain(response.status);
  });
  
  it('should timeout long-running requests', async () => {
    const start = Date.now();
    
    const response = await request(app)
      .post('/api/slow-endpoint')
      .timeout(5000);
    
    const duration = Date.now() - start;
    expect(duration).toBeLessThan(6000);
  });
});
```

## Security Test Utilities

### Test Data Generation
```javascript
// Security test helpers
const SecurityTestUtils = {
  generateMaliciousInputs: () => [
    // SQL Injection
    "'; DROP TABLE users; --",
    "1' OR '1'='1",
    
    // XSS
    '<script>alert("xss")</script>',
    '<img src="x" onerror="alert(1)">',
    
    // Path Traversal
    '../../../etc/passwd',
    '..\\..\\windows\\system32\\config',
    
    // Command Injection
    '; rm -rf /',
    '| cat /etc/passwd',
    
    // LDAP Injection
    '*)(uid=*',
    '*)(&(objectClass=user)(cn=*'
  ],
  
  generateValidInputs: () => [
    'normal@example.com',
    'Test User Name',
    '123-456-7890',
    'https://example.com'
  ],
  
  generateWeakPasswords: () => [
    'password',
    '123456',
    'qwerty',
    'abc123',
    'password123'
  ],
  
  generateStrongPasswords: () => [
    'MyStr0ng!P@ssw0rd',
    'C0mpl3x#P@$$w0rd!',
    'S3cur3!T3st!P@ss'
  ]
};
```

## Test Reporting

### Security Test Report Format
```javascript
const securityTestReport = {
  summary: {
    total_tests: 150,
    passed: 142,
    failed: 5,
    critical_failures: 1,
    high_risk_failures: 2,
    medium_risk_failures: 2
  },
  
  vulnerabilities_found: [
    {
      category: 'Injection',
      severity: 'critical',
      description: 'SQL injection vulnerability in login endpoint',
      remediation: 'Use parameterized queries',
      affected_endpoints: ['/api/auth/login']
    }
  ],
  
  compliance_status: {
    owasp_top_10: 'partial',
    pci_dss: 'compliant',
    gdpr: 'compliant'
  }
};
```

---

*These security testing patterns ensure consistent security validation across all components. All specialists should reference these patterns when implementing security tests.*