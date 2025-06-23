# Security Standards

## Overview
This document defines the unified security standards and patterns used across all specialists in the Claude Template system.

## Security Checklist (Universal)
Every feature must comply with these security requirements:

- [ ] Input validation and sanitization
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS protection (output encoding)
- [ ] CSRF token implementation
- [ ] Rate limiting configuration
- [ ] Authentication/authorization checks
- [ ] Sensitive data encryption
- [ ] Audit logging implementation
- [ ] Error message sanitization
- [ ] Dependency vulnerability scan

## Authentication & Authorization

### Password Security
```javascript
// Secure password handling pattern
const bcrypt = require('bcrypt');
const SALT_ROUNDS = 12;

async function hashPassword(password) {
  // Validate password strength first
  if (!isPasswordStrong(password)) {
    throw new ValidationError('Password does not meet requirements');
  }
  
  // Hash with bcrypt
  return await bcrypt.hash(password, SALT_ROUNDS);
}

async function verifyPassword(password, hash) {
  // Constant-time comparison
  return await bcrypt.compare(password, hash);
}
```

### JWT Token Security
```javascript
// Secure JWT implementation
const jwt = require('jsonwebtoken');

function generateToken(payload) {
  return jwt.sign(payload, process.env.JWT_SECRET, {
    expiresIn: '15m',
    issuer: 'claude-template',
    algorithm: 'HS256'
  });
}

function verifyToken(token) {
  try {
    return jwt.verify(token, process.env.JWT_SECRET, {
      issuer: 'claude-template',
      algorithms: ['HS256']
    });
  } catch (error) {
    throw new AuthenticationError('Invalid token');
  }
}
```

## Input Validation

### SQL Injection Prevention
```javascript
// Use parameterized queries ALWAYS
const query = 'SELECT * FROM users WHERE email = $1 AND active = $2';
const values = [email, true];
const result = await db.query(query, values);

// NEVER use string concatenation
// BAD: `SELECT * FROM users WHERE email = '${email}'`
```

### XSS Prevention
```javascript
// Server-side output encoding
function sanitizeOutput(text) {
  return text
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#x27;');
}

// Use CSP headers
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy', 
    "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'");
  next();
});
```

## OWASP Top 10 Compliance

### A01: Broken Access Control
- Implement role-based access control (RBAC)
- Verify permissions on every protected endpoint
- Use principle of least privilege

### A02: Cryptographic Failures
- Use strong encryption algorithms (AES-256)
- Implement proper key management
- Hash sensitive data with salt

### A03: Injection
- Use parameterized queries
- Validate and sanitize all inputs
- Implement input length limits

### A04: Insecure Design
- Threat modeling for new features
- Security requirements in design phase
- Regular security architecture reviews

### A05: Security Misconfiguration
- Secure default configurations
- Regular security updates
- Remove unnecessary features/accounts

### A06: Vulnerable Components
- Regular dependency updates
- Automated vulnerability scanning
- Component inventory management

### A07: Authentication Failures
- Multi-factor authentication support
- Account lockout mechanisms
- Session management security

### A08: Data Integrity Failures
- Digital signatures for critical data
- Integrity checks for software updates
- Secure serialization practices

### A09: Logging Failures
- Security event logging
- Log integrity protection
- Real-time monitoring and alerting

### A10: Server-Side Request Forgery
- URL validation and allowlisting
- Network segmentation
- Input validation for URLs

## Security Testing Requirements

### Static Analysis
```yaml
tools:
  - ESLint security plugins
  - Semgrep for security patterns
  - git-secrets for credential scanning
  - SonarQube for code quality
```

### Dynamic Testing
```yaml
requirements:
  - Penetration testing for new features
  - Automated security scans in CI/CD
  - Regular vulnerability assessments
  - Security regression testing
```

## Audit Logging

### Security Event Logging
```javascript
// Security event logging pattern
const auditLogger = require('./audit-logger');

function logSecurityEvent(event, details) {
  auditLogger.security({
    event_type: event,
    timestamp: new Date().toISOString(),
    user_id: details.userId,
    session_id: details.sessionId,
    ip_address: details.ipAddress,
    user_agent: details.userAgent,
    details: details.eventDetails,
    risk_level: details.riskLevel || 'medium'
  });
}

// Usage examples
logSecurityEvent('LOGIN_SUCCESS', { userId: '123', ipAddress: req.ip });
logSecurityEvent('FAILED_LOGIN_ATTEMPT', { email: 'user@example.com', ipAddress: req.ip });
logSecurityEvent('PRIVILEGE_ESCALATION', { userId: '123', oldRole: 'user', newRole: 'admin' });
```

## Error Handling

### Secure Error Messages
```javascript
// Don't expose internal details
function sanitizeError(error) {
  if (process.env.NODE_ENV === 'production') {
    return {
      message: 'An error occurred',
      request_id: generateRequestId()
    };
  }
  
  // Development mode can show more details
  return {
    message: error.message,
    stack: error.stack,
    request_id: generateRequestId()
  };
}
```

## Deployment Security

### Environment Configuration
```yaml
security_headers:
  - X-Content-Type-Options: nosniff
  - X-Frame-Options: DENY
  - X-XSS-Protection: 1; mode=block
  - Strict-Transport-Security: max-age=31536000; includeSubDomains
  - Referrer-Policy: strict-origin-when-cross-origin

secrets_management:
  - Use environment variables for secrets
  - Rotate secrets regularly
  - Never commit secrets to version control
  - Use dedicated secret management tools
```

## Compliance Requirements

### Data Protection
- GDPR compliance for EU users
- Data minimization principles
- User consent management
- Right to erasure implementation

### Industry Standards
- SOC 2 Type II controls
- ISO 27001 security framework
- PCI DSS for payment processing
- HIPAA for healthcare data

## Emergency Response

### Incident Response Plan
1. **Detection**: Automated monitoring alerts
2. **Assessment**: Determine scope and impact  
3. **Containment**: Isolate affected systems
4. **Eradication**: Remove root cause
5. **Recovery**: Restore normal operations
6. **Lessons Learned**: Update procedures

### Security Contacts
- Security team: security@company.com
- Incident response: incident@company.com
- External security researcher: security-research@company.com

---

*This document serves as the single source of truth for security standards. All specialists must reference these patterns rather than implementing their own security measures.*