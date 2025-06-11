# Common Validation Patterns

## Input Validation Framework

### Basic Validators

```javascript
// Email validation
const validateEmail = (email) => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return {
    valid: emailRegex.test(email),
    error: emailRegex.test(email) ? null : "Invalid email format"
  };
};

// URL validation
const validateURL = (url) => {
  try {
    new URL(url);
    return { valid: true, error: null };
  } catch {
    return { valid: false, error: "Invalid URL format" };
  }
};

// UUID validation
const validateUUID = (uuid) => {
  const uuidRegex = /^[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$/i;
  return {
    valid: uuidRegex.test(uuid),
    error: uuidRegex.test(uuid) ? null : "Invalid UUID format"
  };
};
```

### String Validators

```javascript
// Length validation
const validateLength = (value, min, max) => {
  const length = value.length;
  if (length < min) {
    return { valid: false, error: `Minimum length is ${min}` };
  }
  if (length > max) {
    return { valid: false, error: `Maximum length is ${max}` };
  }
  return { valid: true, error: null };
};

// Pattern validation
const validatePattern = (value, pattern, errorMessage) => {
  return {
    valid: pattern.test(value),
    error: pattern.test(value) ? null : errorMessage
  };
};

// Required field validation
const validateRequired = (value) => {
  const isValid = value !== null && 
                  value !== undefined && 
                  value !== '' &&
                  (typeof value !== 'string' || value.trim() !== '');
  return {
    valid: isValid,
    error: isValid ? null : "This field is required"
  };
};
```

### Number Validators

```javascript
// Range validation
const validateRange = (value, min, max) => {
  const num = Number(value);
  if (isNaN(num)) {
    return { valid: false, error: "Must be a number" };
  }
  if (num < min || num > max) {
    return { valid: false, error: `Must be between ${min} and ${max}` };
  }
  return { valid: true, error: null };
};

// Integer validation
const validateInteger = (value) => {
  const num = Number(value);
  return {
    valid: Number.isInteger(num),
    error: Number.isInteger(num) ? null : "Must be an integer"
  };
};

// Positive number validation
const validatePositive = (value) => {
  const num = Number(value);
  return {
    valid: num > 0,
    error: num > 0 ? null : "Must be a positive number"
  };
};
```

## Schema Validation

### Object Schema Validator

```javascript
const validateSchema = (data, schema) => {
  const errors = {};
  let isValid = true;

  for (const [field, rules] of Object.entries(schema)) {
    const value = data[field];
    const fieldErrors = [];

    // Check each rule
    for (const rule of rules) {
      const result = rule.validate(value);
      if (!result.valid) {
        fieldErrors.push(result.error);
        isValid = false;
      }
    }

    if (fieldErrors.length > 0) {
      errors[field] = fieldErrors;
    }
  }

  return { valid: isValid, errors };
};

// Example usage
const userSchema = {
  email: [
    { validate: validateRequired },
    { validate: validateEmail }
  ],
  age: [
    { validate: validateRequired },
    { validate: (v) => validateRange(v, 18, 120) }
  ],
  username: [
    { validate: validateRequired },
    { validate: (v) => validateLength(v, 3, 20) },
    { validate: (v) => validatePattern(v, /^[a-zA-Z0-9_]+$/, "Only letters, numbers, and underscores") }
  ]
};
```

## API Request Validation

### Request Body Validation

```javascript
const validateRequestBody = (req, schema) => {
  const validation = validateSchema(req.body, schema);
  
  if (!validation.valid) {
    return {
      valid: false,
      status: 400,
      error: {
        code: "VALIDATION_ERROR",
        message: "Invalid request data",
        details: validation.errors
      }
    };
  }
  
  return { valid: true };
};
```

### Query Parameter Validation

```javascript
const validateQueryParams = (params, allowed) => {
  const unknown = Object.keys(params).filter(key => !allowed.includes(key));
  
  if (unknown.length > 0) {
    return {
      valid: false,
      error: `Unknown parameters: ${unknown.join(', ')}`
    };
  }
  
  return { valid: true };
};
```

## File Validation

### File Type Validation

```javascript
const validateFileType = (file, allowedTypes) => {
  const extension = file.name.split('.').pop().toLowerCase();
  const mimeType = file.type;
  
  const isValidExtension = allowedTypes.extensions?.includes(extension);
  const isValidMime = allowedTypes.mimeTypes?.includes(mimeType);
  
  return {
    valid: isValidExtension || isValidMime,
    error: (isValidExtension || isValidMime) ? null : 
           `File type not allowed. Allowed types: ${allowedTypes.extensions?.join(', ')}`
  };
};
```

### File Size Validation

```javascript
const validateFileSize = (file, maxSizeInBytes) => {
  return {
    valid: file.size <= maxSizeInBytes,
    error: file.size <= maxSizeInBytes ? null :
           `File too large. Maximum size: ${(maxSizeInBytes / 1024 / 1024).toFixed(2)}MB`
  };
};
```

## Security Validation

### SQL Injection Prevention

```javascript
const validateSQLSafe = (value) => {
  const sqlKeywords = /(\b(SELECT|INSERT|UPDATE|DELETE|DROP|UNION|ALTER|CREATE)\b)/gi;
  const sqlSpecialChars = /[;'"\\]/g;
  
  const hasSQLKeywords = sqlKeywords.test(value);
  const hasSpecialChars = sqlSpecialChars.test(value);
  
  return {
    valid: !hasSQLKeywords && !hasSpecialChars,
    error: (hasSQLKeywords || hasSpecialChars) ? "Potentially unsafe input detected" : null
  };
};
```

### XSS Prevention

```javascript
const validateXSSSafe = (value) => {
  const xssPatterns = [
    /<script[^>]*>.*?<\/script>/gi,
    /<iframe[^>]*>.*?<\/iframe>/gi,
    /javascript:/gi,
    /on\w+\s*=/gi
  ];
  
  const hasXSS = xssPatterns.some(pattern => pattern.test(value));
  
  return {
    valid: !hasXSS,
    error: hasXSS ? "Potentially unsafe HTML detected" : null
  };
};
```

## Composite Validators

### Password Validation

```javascript
const validatePassword = (password) => {
  const rules = [
    { test: /.{8,}/, message: "At least 8 characters" },
    { test: /[A-Z]/, message: "At least one uppercase letter" },
    { test: /[a-z]/, message: "At least one lowercase letter" },
    { test: /[0-9]/, message: "At least one number" },
    { test: /[^A-Za-z0-9]/, message: "At least one special character" }
  ];
  
  const failedRules = rules.filter(rule => !rule.test.test(password));
  
  return {
    valid: failedRules.length === 0,
    errors: failedRules.map(rule => rule.message),
    strength: (rules.length - failedRules.length) / rules.length
  };
};
```

### Date Validation

```javascript
const validateDate = (dateString, options = {}) => {
  const date = new Date(dateString);
  
  if (isNaN(date.getTime())) {
    return { valid: false, error: "Invalid date format" };
  }
  
  if (options.minDate && date < new Date(options.minDate)) {
    return { valid: false, error: `Date must be after ${options.minDate}` };
  }
  
  if (options.maxDate && date > new Date(options.maxDate)) {
    return { valid: false, error: `Date must be before ${options.maxDate}` };
  }
  
  if (options.futureOnly && date <= new Date()) {
    return { valid: false, error: "Date must be in the future" };
  }
  
  if (options.pastOnly && date >= new Date()) {
    return { valid: false, error: "Date must be in the past" };
  }
  
  return { valid: true, error: null };
};
```

## Validation Middleware

### Express Middleware Example

```javascript
const validationMiddleware = (schema) => {
  return (req, res, next) => {
    const validation = validateSchema(req.body, schema);
    
    if (!validation.valid) {
      return res.status(400).json({
        error: "Validation failed",
        details: validation.errors
      });
    }
    
    // Attach validated data
    req.validated = req.body;
    next();
  };
};

// Usage
app.post('/api/users', 
  validationMiddleware(userSchema), 
  (req, res) => {
    // req.validated contains validated data
  }
);
```

## Testing Validation

### Validation Test Patterns

```javascript
describe('Email Validation', () => {
  const validEmails = [
    'user@example.com',
    'test.user+tag@example.co.uk',
    'name@subdomain.example.com'
  ];
  
  const invalidEmails = [
    'notanemail',
    '@example.com',
    'user@',
    'user @example.com',
    'user@example'
  ];
  
  validEmails.forEach(email => {
    it(`should accept valid email: ${email}`, () => {
      const result = validateEmail(email);
      expect(result.valid).toBe(true);
      expect(result.error).toBeNull();
    });
  });
  
  invalidEmails.forEach(email => {
    it(`should reject invalid email: ${email}`, () => {
      const result = validateEmail(email);
      expect(result.valid).toBe(false);
      expect(result.error).toBeTruthy();
    });
  });
});
```

## Best Practices

1. **Validate Early**: Check input as soon as it enters the system
2. **Be Specific**: Provide clear, actionable error messages
3. **Layer Validation**: Use client-side for UX, server-side for security
4. **Sanitize After Validation**: Clean data after validating
5. **Log Validation Failures**: Track patterns for security monitoring
6. **Performance Consider**: Cache regex compilation for frequently used patterns
7. **Fail Securely**: Reject invalid input rather than trying to fix it
