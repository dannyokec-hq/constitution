# Code Culture at Dannyokec

This document outlines Dannyokec's coding culture, standards, and practices, with a strong emphasis on security and quantum organization principles.

## Core Principles

1. **Security First**
   - Security is integrated into every aspect of development
   - Regular security training and awareness
   - Proactive security measures
   - Continuous security monitoring

2. **Quantum Organization Mindset**
   - Consider all possible scenarios
   - Plan for edge cases
   - Document potential risks
   - Implement robust error handling

3. **Clean Code**
   - Write self-documenting code
   - Follow SOLID principles
   - Keep functions small and focused
   - Use meaningful variable and function names

## Coding Standards

### Security-Focused Guidelines

1. **Input Validation**
   ```javascript
   // Good
   const sanitizeInput = (input) => {
     if (typeof input !== 'string') {
       throw new Error('Invalid input type');
     }
     return input.replace(/[<>]/g, '');
   };

   // Bad
   const processInput = (input) => {
     return input;
   };
   ```
2. **Database Schema Documentation**
   ```markdown
   # Database Schema Documentation

   ## Overview
   This section documents our database schemas, detailing table structures, relationships, and the purpose of each column.

   ## Tables

   ### Users Table
   Stores core user information and authentication details.

   #### Columns:
   1. `id` (BIGINT UNSIGNED AUTO_INCREMENT)
      - Primary key
      - Unique identifier for each user
   2. `username` (VARCHAR(50))
      - Unique username for login
      - Required, indexed
   3. `email` (VARCHAR(255)) 
      - User's email address
      - Unique, indexed
   4. `password_hash` (VARCHAR(255))
      - Bcrypt hashed password
   5. `created_at` (DATETIME)
      - Timestamp of account creation
   6. `updated_at` (DATETIME) 
      - Last modification timestamp

   ### Transactions Table
   Records all financial transactions in the system.

   #### Columns:
   1. `id` (BIGINT UNSIGNED AUTO_INCREMENT)
      - Primary key
      - Unique transaction identifier
   2. `user_id` (BIGINT UNSIGNED)
      - Foreign key to users table
   3. `amount` (DECIMAL(15,2))
      - Transaction amount
   4. `type` (ENUM)
      - Transaction type (deposit/withdrawal/transfer)
   5. `status` (ENUM)
      - Current status (pending/completed/failed)
   6. `created_at` (DATETIME)
      - Transaction timestamp

   ### Products Table
   Manages available products/services.

   #### Columns:
   1. `id` (BIGINT UNSIGNED AUTO_INCREMENT)
      - Primary key
      - Unique product identifier  
   2. `name` (VARCHAR(100))
      - Product name
   3. `description` (TEXT)
      - Detailed product description
   4. `price` (DECIMAL(10,2))
      - Current product price
   5. `active` (BOOLEAN)
      - Product availability status
   ```

   Each table includes:
   - Clear column definitions
   - Data types with size/precision
   - Purpose descriptions
   - Key relationships
   - Indexing considerations

2. **Secure Authentication**
   ```php
   // Good
   function authenticateUser(string $username, string $password): bool 
   {
       if (empty($username) || empty($password)) {
           return false;
       }
       $hashedPassword = password_hash($password, PASSWORD_BCRYPT);
       return password_verify($password, $storedHash);
   }

   // Bad
   function login($username, $password)
   {
       return $username === "admin" && $password === "password123";
   }
   ```

3. **Environment Variables**
   ```php
   // Good
   require_once __DIR__ . '/vendor/autoload.php';
   $dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
   $dotenv->load();
   
   $apiKey = $_ENV['API_KEY'];
   $dbPassword = $_ENV['DB_PASSWORD'];

   // Bad
   $apiKey = "1234567890abcdef";
   $dbPassword = "password123";
   ```

4. **Secure File Operations**
   ```php
   // Good
   function saveFile(string $filename, string $content): void
   {
       // Generate random filename with safe extension
       $extension = '.txt'; // Force safe extension
       $randomName = bin2hex(random_bytes(16)) . $extension;
       
       $targetDir = __DIR__ . '/safe_directory';
       $safePath = $targetDir . DIRECTORY_SEPARATOR . $randomName;
       
       // Validate content mime type
       $finfo = new finfo(FILEINFO_MIME_TYPE);
       $mimeType = $finfo->buffer($content);
       
       $allowedMimes = ['text/plain'];
       if (!in_array($mimeType, $allowedMimes)) {
           throw new RuntimeException('Invalid file type');
       }
       
       if (!is_dir($targetDir)) {
           mkdir($targetDir, 0755, true);
       }
       
       file_put_contents($safePath, $content, LOCK_EX);
   }

   // Bad
   function saveFile($filename, $content)
   {
       file_put_contents($filename, $content);
   }
   ```

5. **API Security**
   ```php
   // Good
   use Slim\Factory\AppFactory;
   
   $app = AppFactory::create();
   
   // Rate limiting middleware
   $app->add(new RateLimit\RateLimitMiddleware([
       'interval' => 900, // 15 minutes
       'requests' => 100 // limit each IP to 100 requests per interval
   ]));
   
   // Security headers middleware
   $app->add(function ($request, $handler) {
       $response = $handler->handle($request);
       return $response
           ->withHeader('X-Frame-Options', 'DENY')
           ->withHeader('X-XSS-Protection', '1; mode=block')
           ->withHeader('X-Content-Type-Options', 'nosniff');
   });

   // Bad
   $app->get('/api/data', function ($request, $response) {
       return $response->withJson($sensitiveData);
   });
   ```
   ```

### Language-Specific Guidelines

#### JavaScript/TypeScript
```typescript
// Good
interface User {
  id: string;
  email: string;
  role: 'admin' | 'user' | 'guest';
}

const validateUser = (user: User): boolean => {
  if (!user.id || !user.email) {
    return false;
  }
  return true;
};

// Bad
const checkUser = (user) => {
  return user.id;
};
```
#### php
```php
// Good
class User {
    private string $id;
    private string $email;
    private array $permissions;

    public function __construct(string $id, string $email, array $permissions) {
        $this->id = $id;
        $this->email = $email;
        $this->permissions = $permissions;
    }

    public function hasPermission(string $permission): bool {
        return in_array($permission, $this->permissions);
    }
}

// Bad
class User {
    public $id;
    public $email;
}
```
#### Python
```python
# Good
from typing import List, Optional
from dataclasses import dataclass

@dataclass
class User:
    id: str
    email: str
    role: str
    permissions: List[str]

    def has_permission(self, permission: str) -> bool:
        return permission in self.permissions

# Bad
class User:
    def __init__(self, id, email):
        self.id = id
        self.email = email
```

## Git Workflow & Branch Strategy

1. **Core Branches**
   - `production` - Live production code
   - `staging` - Pre-production testing
   - `development` - Integration branch for features

2. **Branch Flow**
   Examples of branch names:
   - `feature/security/add-2fa` - Adding two-factor authentication
   - `feature/security/password-reset` - Password reset functionality
   - `feature/auth/login-page` - New login page UI
   - `bugfix/api/rate-limit` - Fix rate limiting issue
   - `feature/payments/stripe-integration` - Adding Stripe payment
   - `hotfix/security/xss-patch` - Patch XSS vulnerability

   ```
   feature/security/description
        ↓
   development
        ↓
   staging
        ↓
   production
   ```

3. **Branch Permissions**
   - Developers can only merge to `development`
   - `staging` can only merge from `development`
   - `production` can only merge from `staging`
   - All merges require minimum two approving reviews

4. **Branch Protection Rules**
   - No direct commits to `production`, `staging`, or `development`
   - Required status checks must pass
   - Branch must be up to date before merging
   - Enforce linear history

## Code Review Process

1. **Review Requirements**
   - Minimum two approving reviews required
   - One review must be from a senior developer
   - Security-critical changes require security team review
   - All comments must be resolved before merge
   - CODEOWNERS file enforces team-based reviews:
     - Development changes require `@org/okecbot_app-development-senior` approval
     - Staging promotions require `@org/okecbot_app-development-lead` approval
     - Production deployments require `@org/okecbot_app-production-lead` approval
     - Security changes require additional `@org/okecbot_app-security-senior` approval
     - Team members can belong to multiple teams based on role and seniority
     - Example team structure:
       ```
       okecbot_app-development-junior
       okecbot_app-development-senior  
       okecbot_app-development-lead
       okecbot_app-security-senior
       okecbot_app-production-lead
       ```

2. **Review Flow**
   ```
   Create PR to development
        ↓
   Automated checks run
        ↓
   First reviewer approval
        ↓
   Second reviewer approval
        ↓
   Merge to development
        ↓
   Stage promotion (by authorized team)
        ↓
   Production deployment (by authorized team)
   ```

3. **Review Checklist**
   - Input validation
   - Authentication and authorization
   - Data encryption
   - Error handling
   - Logging practices
   - API security
   - Dependency security

## Documentation Standards

1. **Security Documentation**
   - Document security measures
   - Include threat models where applicable
   - Document attack vectors and risks
   - Security testing procedures based on team size:
     - For teams with 2 or fewer contributors:
       - One reviewer approval required
       - Security changes still require security team review
     - For teams with 3+ contributors:
       - Standard two reviewer requirement applies

2. **API Documentation**
   - Document security requirements
   - Authentication methods
   - Rate limiting
   - Error handling
   - Data validation

3. **System Documentation**
   - Security architecture
   - Access control
   - Data flow
   - Encryption methods

## Best Practices

1. **Security Testing**
   - Regular penetration testing
   - Vulnerability scanning
   - Security code reviews
   - Dependency scanning

2. **Performance & Security**
   - Optimize database queries
   - Implement caching securely
   - Monitor resource usage
   - Profile code regularly

3. **Code Formatting & Linting**
   - Use Prettier extension with the following settings:
     ```json
     {
       "printWidth": 120,
       "tabWidth": 2,
       "useTabs": false,
       "semi": true,
       "singleQuote": true,
       "trailingComma": "es5",
       "bracketSpacing": true,
       "arrowParens": "avoid",
       "requirePragma": false,
       "insertPragma": false,
       "proseWrap": "preserve",
       "endOfLine": "lf",
       "embeddedLanguageFormatting": "auto"
     }
     ```
   - ESLint configuration must enforce:
     ```json
     {
       "parserOptions": {
         "ecmaVersion": 6,
         "sourceType": "script"
       },
       "rules": {
         "prefer-const": "error",
         "no-var": "error",
         "padding-line-between-statements": [
           "error",
           { "blankLine": "always", "prev": ["block-like", "function"], "next": "*" },
           { "blankLine": "always", "prev": "*", "next": ["block-like", "function"] }
         ]
       }
     }
     ```
   - Format on save must be enabled
   - Run linting in CI/CD pipeline
   - No commits should contain linting errors
   - ES6 with require() statements preferred over import
   - Maintain 2 blank lines before and after code blocks

4. **Compliance**
   - Follow industry standards
   - Document compliance measures
   - Regular audits
   - Update procedures

## Tools and Automation

1. **Security Tools**
   - Static code analysis
   - Dynamic scanning
   - Dependency checking
   - Security monitoring

2. **CI/CD Security**
   - Automated security testing
   - Vulnerability scanning
   - Security checks
   - Secure deployment

## Learning and Growth

1. **Security Training**
   - Regular security workshops
   - Capture the flag events
   - Security certifications
   - Knowledge sharing

2. **Continuous Improvement**
   - Security reviews
   - Process optimization
   - Tool evaluation
   - Best practice updates 