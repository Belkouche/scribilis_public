# Scribilis Security Architecture Documentation

## Table of Contents
1. [Authentication & JWT Implementation](#authentication--jwt-implementation)
2. [Role-Based Access Control (RBAC)](#role-based-access-control-rbac)
3. [Input Validation & Sanitization](#input-validation--sanitization)
4. [Data Protection & Privacy](#data-protection--privacy)
5. [API Security](#api-security)
6. [Content Security & Moderation](#content-security--moderation)
7. [Infrastructure Security](#infrastructure-security)
8. [Compliance & Regulations](#compliance--regulations)

---

## Authentication & JWT Implementation

### JWT Token Structure

Scribilis uses a secure JWT-based authentication system with the following payload structure:

```typescript
interface JWTPayload {
  userId: string;         // Unique user identifier
  email: string;          // User email address
  role: 'PARENT' | 'CHILD' | 'ADMIN';  // User role for RBAC
  childId?: string;       // Child ID if user is a child
  parentId?: string;      // Parent ID if user is associated with a parent
}
```

### Security Features

#### 1. **Environment-Based Secret Management**
```typescript
// Critical: JWT secret must be provided via environment variable
const JWT_SECRET = process.env.JWT_SECRET;
if (!JWT_SECRET) {
  throw new Error('JWT_SECRET environment variable is required');
}
```

- **No Fallback Values**: Application refuses to start without proper JWT secret
- **Secret Generation**: `openssl rand -base64 32` recommended for production
- **Environment Isolation**: Different secrets per environment (dev/staging/prod)

#### 2. **Token Expiration & Lifecycle**
```typescript
// Standard authentication tokens
signToken(payload): string {
  return jwt.sign(payload, JWT_SECRET, { expiresIn: '24h' });
}

// Magic link tokens (passwordless authentication)
generateMagicLinkToken(email): string {
  return jwt.sign({ email, type: 'magic-link' }, JWT_SECRET, { expiresIn: '10m' });
}
```

- **24-hour expiry** for regular authentication tokens
- **10-minute expiry** for magic link tokens
- **Automatic invalidation** on token expiry
- **Secure token verification** with error handling

#### 3. **Password Security**
```typescript
// BCrypt with 12 salt rounds for password hashing
async function hashPassword(password: string): Promise<string> {
  return bcrypt.hash(password, 12);
}

async function verifyPassword(password: string, hashedPassword: string): Promise<boolean> {
  return bcrypt.compare(password, hashedPassword);
}
```

- **BCrypt with 12 salt rounds** (industry standard for high security)
- **Salted hashing** prevents rainbow table attacks
- **Async operations** prevent blocking the event loop

#### 4. **Multi-Channel Token Delivery**
```typescript
function getTokenFromRequest(request: NextRequest): string | null {
  // Check Authorization header first
  const authHeader = request.headers.get('authorization');
  if (authHeader && authHeader.startsWith('Bearer ')) {
    return authHeader.substring(7);
  }

  // Fallback to HTTP-only cookies
  const token = request.cookies.get('auth-token')?.value;
  return token || null;
}
```

- **Bearer tokens** in Authorization header for API calls
- **HTTP-only cookies** for web application security
- **No localStorage/sessionStorage** to prevent XSS token theft

---

## Role-Based Access Control (RBAC)

### User Role Hierarchy

```
ADMIN
├── Full platform access
├── User management
├── Content moderation
├── System configuration
└── Analytics access

PARENT
├── Child account management
├── Progress monitoring
├── Subscription management
├── Content review
└── Sharing permissions

CHILD
├── Game access
├── Story creation
├── Own profile management (limited)
└── Own content access only
```

### Granular Permissions System

#### Parent-Child Access Control
```typescript
interface RBACPermissions {
  canModifyProfile: boolean;      // Edit child's profile information
  canViewProgress: boolean;       // View stories, progress, achievements
  canManageSharing: boolean;      // Manage sharing with other parents
  canDeleteAccount: boolean;      // Delete child's account
}
```

#### Access Verification Functions
```typescript
// Verify specific permission for parent-child relationship
async function verifyChildAccess(
  parentUserId: string,
  childId: string,
  requiredPermission: keyof RBACPermissions
): Promise<boolean>

// Child can only access their own data
async function verifyChildSelfAccess(
  childUserId: string,
  requestedChildId: string
): Promise<boolean>
```

### Security Features

#### 1. **Database-Level Permission Checks**
- All permissions stored in `ParentChild` relationship table
- **No client-side permission logic** - all checks server-side
- **Fail-secure approach** - deny access on any error

#### 2. **Data Filtering Based on Permissions**
```typescript
async function getChildDataWithPermissions(parentUserId: string, childId: string) {
  // Only return data fields parent has permission to view
  const childData = await prisma.child.findUnique({
    select: {
      id: true,
      username: true,
      firstName: true,
      lastName: childAccess.permissions.canModifyProfile ? true : false,
      birthDate: childAccess.permissions.canModifyProfile ? true : false,
      // ...selective field inclusion based on permissions
    }
  });
}
```

#### 3. **Audit Logging for GDPR Compliance**
```typescript
async function logChildDataAccess(
  parentUserId: string,
  childId: string,
  action: string,
  metadata?: any
) {
  // Comprehensive audit trail for all child data access
  console.log('CHILD_DATA_ACCESS_AUDIT', {
    timestamp: new Date().toISOString(),
    parentUserId,
    childId,
    action,
    metadata,
    source: 'RBAC_SYSTEM'
  });
}
```

---

## Input Validation & Sanitization

### Zod-Based Schema Validation

#### 1. **Comprehensive Input Schemas**
```typescript
// Email validation with normalization
export const emailSchema = z.string()
  .email('Format d\'email invalide')
  .toLowerCase();

// Password security requirements
export const passwordSchema = z.string()
  .min(8, 'Le mot de passe doit contenir au moins 8 caractères')
  .max(128, 'Le mot de passe ne peut pas dépasser 128 caractères')
  .regex(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/,
    'Le mot de passe doit contenir au moins une majuscule, une minuscule et un chiffre');

// Username with character restrictions
export const usernameSchema = z.string()
  .min(3, 'Le nom d\'utilisateur doit contenir au moins 3 caractères')
  .max(20, 'Le nom d\'utilisateur ne peut pas dépasser 20 caractères')
  .regex(/^[a-zA-Z0-9_-]+$/, 'Lettres, chiffres, tirets et underscores uniquement');
```

#### 2. **Validation Middleware**
```typescript
// Automatic validation for API routes
export async function validateRequestBody<T>(
  request: NextRequest,
  schema: ZodSchema<T>
): Promise<ValidationResult<T>>

// Higher-order function for complete request validation
export function withValidation<TBody, TQuery, TParams>(
  handler: Function,
  options?: {
    body?: ZodSchema<TBody>;
    query?: ZodSchema<TQuery>;
    params?: ZodSchema<TParams>;
  }
)
```

#### 3. **XSS Prevention**
```typescript
export function sanitizeHtmlContent(content: string): string {
  return content
    .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')  // Remove scripts
    .replace(/\son\w+\s*=\s*["'][^"']*["']/gi, '')                      // Remove on* attributes
    .replace(/\son\w+\s*=\s*[^\s>]*/gi, '')
    .replace(/javascript:/gi, '')                                        // Remove javascript: URLs
    .replace(/vbscript:/gi, '')                                         // Remove vbscript: URLs
    .trim();
}
```

### Validation Layers

#### 1. **Client-Side Validation** (User Experience)
- Immediate feedback with React Hook Form
- **Not trusted for security** - purely for UX

#### 2. **API Route Validation** (Security Layer)
- **Zod schema validation** on all inputs
- **Type-safe validation** with TypeScript integration
- **Comprehensive error messages** in French

#### 3. **Database Constraints** (Final Security Layer)
- Prisma schema constraints
- Database-level foreign key constraints
- **Unique constraints** for usernames and emails

---

## Data Protection & Privacy

### Child Data Protection (COPPA/GDPR Compliant)

#### 1. **Minimal Data Collection**
```typescript
// Only collect necessary data for children
export const createChildSchema = z.object({
  username: usernameSchema,           // Required for login
  firstName: nameSchema,              // Required for personalization
  lastName: nameSchema.optional(),    // Optional - reduces PII
  birthDate: birthDateSchema,         // Age verification only
  grade: gradeSchema,                 // Educational level for content
  password: passwordSchema            // Secure authentication
});
```

#### 2. **Age Verification**
```typescript
export const birthDateSchema = z.string()
  .datetime('Format de date invalide')
  .refine((date) => {
    const birthDate = new Date(date);
    const age = now.getFullYear() - birthDate.getFullYear();
    return age >= 5 && age <= 18;  // Platform age limits
  }, 'L\'âge doit être entre 5 et 18 ans');
```

#### 3. **Data Anonymization**
- **No real names required** for children (username system)
- **Optional last names** to reduce PII
- **Age ranges instead of exact birthdates** for analytics

### Encryption & Storage

#### 1. **Password Security**
- **BCrypt hashing** with 12 salt rounds
- **No plaintext password storage**
- **Secure password reset** via time-limited tokens

#### 2. **Sensitive Data Handling**
- **PII encrypted at rest** (future implementation)
- **HTTPS-only** communication
- **Secure cookie attributes** (HttpOnly, Secure, SameSite)

---

## API Security

### Authentication Middleware

#### 1. **Request Authentication**
```typescript
export function verifyAuth(request: NextRequest): JWTPayload | null {
  const token = getTokenFromRequest(request);
  if (!token) return null;
  return verifyToken(token);
}

export function requireAuth(requiredRoles?: ('PARENT' | 'CHILD' | 'ADMIN')[]) {
  return (request: NextRequest) => {
    const auth = verifyAuth(request);
    if (!auth) {
      return NextResponse.json({ error: 'Non autorisé - Connexion requise' }, { status: 401 });
    }
    if (requiredRoles && !requiredRoles.includes(auth.role)) {
      return NextResponse.json({ error: 'Accès refusé - Permissions insuffisantes' }, { status: 403 });
    }
    return null; // Auth successful
  };
}
```

#### 2. **Rate Limiting** (Planned)
```typescript
export function validateRateLimit(
  request: NextRequest,
  options?: {
    maxRequests?: number;    // Maximum requests per window
    windowMs?: number;       // Time window in milliseconds
    identifier?: string;     // Custom identifier (IP, user ID, etc.)
  }
): boolean
```

### Request Validation

#### 1. **Content-Length Protection**
```typescript
export function validateContentLength(
  request: NextRequest,
  maxBytes: number = 1024 * 1024  // 1MB default
): boolean
```

#### 2. **MIME Type Validation**
```typescript
export function validateContentType(
  request: NextRequest,
  allowedTypes: string[]
): boolean
```

---

## Content Security & Moderation

### AI-Powered Content Filtering

#### 1. **Multi-Provider Moderation**
```typescript
// Each AI provider implements content moderation
interface UniversalAIProvider {
  moderateContent(text: string): Promise<{
    flagged: boolean;
    categories: Record<string, boolean>;
    categoryScores: Record<string, number>;
  }>;
}
```

#### 2. **Configurable Moderation Levels**
```typescript
// Admin-configurable moderation strictness
moderationLevel: z.enum(['lenient', 'moderate', 'strict'])

// Lenient: Allows creative fantasy content
// Moderate: Balanced creativity and safety
// Strict: Educational content only
```

#### 3. **Real-Time Content Analysis**
- **Pre-publication scanning** of all user-generated content
- **AI-powered detection** of inappropriate themes
- **Automatic flagging** for manual review
- **Context-aware filtering** for age-appropriate content

### Content Safety Features

#### 1. **Automated Content Review**
- **Real-time AI moderation** during story creation
- **Keyword filtering** for inappropriate language
- **Context analysis** for age-appropriate themes

#### 2. **Manual Review Workflow**
- **Admin moderation queue** for flagged content
- **24-hour review commitment** for flagged content
- **Appeals process** for content restoration

---

## Infrastructure Security

### Network Security

#### 1. **HTTPS Enforcement**
- **TLS 1.2+ only** for all communications
- **HSTS headers** for HTTPS enforcement
- **Secure cookie attributes** (Secure, HttpOnly, SameSite)

#### 2. **CORS Configuration**
```typescript
// Strict CORS policy for API endpoints
const corsHeaders = {
  'Access-Control-Allow-Origin': process.env.ALLOWED_ORIGINS || 'https://scribilis.com',
  'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
  'Access-Control-Allow-Headers': 'Content-Type, Authorization',
  'Access-Control-Allow-Credentials': 'true'
};
```

### Database Security

#### 1. **Prisma ORM Protection**
- **SQL injection prevention** through parameterized queries
- **Type-safe database operations**
- **Connection pooling** and timeout management

#### 2. **Database Access Control**
- **Least privilege** database user permissions
- **Connection string encryption** in environment variables
- **Database-level constraints** and foreign keys

---

## Compliance & Regulations

### GDPR Compliance

#### 1. **Data Subject Rights**
- **Right to access**: Users can export their data
- **Right to rectification**: Users can modify their information
- **Right to erasure**: Account deletion removes all associated data
- **Right to portability**: Data export in machine-readable format

#### 2. **Privacy by Design**
- **Data minimization**: Only collect necessary information
- **Purpose limitation**: Data used only for stated purposes
- **Storage limitation**: Automatic data retention policies
- **Accountability**: Comprehensive audit logging

### COPPA Compliance

#### 1. **Parental Consent**
- **Verifiable parental consent** for child accounts
- **Parent-controlled permissions** for data sharing
- **Limited data collection** for children under 13

#### 2. **Child Data Protection**
- **No behavioral advertising** to children
- **Restricted data sharing** with third parties
- **Enhanced security** for child accounts

### Security Monitoring

#### 1. **Audit Logging**
```typescript
// Comprehensive audit trail for all sensitive operations
export async function logChildDataAccess(
  parentUserId: string,
  childId: string,
  action: string,
  metadata?: any
) {
  // GDPR Article 30 compliant logging
}
```

#### 2. **Security Metrics**
- **Failed login attempt monitoring**
- **Unusual access pattern detection**
- **Content moderation effectiveness tracking**
- **System performance and availability monitoring**

---

## Security Best Practices Implementation

### Development Security

#### 1. **Secure Coding Standards**
- **Input validation** on all user inputs
- **Output encoding** to prevent XSS
- **Error handling** without information disclosure
- **Secure configuration** management

#### 2. **Testing & Quality Assurance**
- **Unit tests** for security functions
- **Integration tests** for authentication flows
- **Security vulnerability scanning**
- **Regular dependency updates**

### Deployment Security

#### 1. **Environment Security**
- **Secret management** via environment variables
- **No hardcoded credentials** in source code
- **Secure deployment pipelines**
- **Production environment isolation**

#### 2. **Monitoring & Incident Response**
- **Real-time security monitoring**
- **Automated vulnerability detection**
- **Incident response procedures**
- **Regular security audits**

---

## Future Security Enhancements

### Planned Improvements

#### 1. **Advanced Authentication**
- **Multi-factor authentication** (MFA) for parents
- **Biometric authentication** support
- **Social login** with privacy controls
- **Session management** improvements

#### 2. **Enhanced Privacy**
- **End-to-end encryption** for sensitive data
- **Advanced anonymization** techniques
- **Differential privacy** for analytics
- **Zero-knowledge architecture** exploration

#### 3. **AI Security**
- **Adversarial attack protection** for AI models
- **Model privacy** preservation
- **Bias detection** and mitigation
- **Explainable AI** for moderation decisions

This comprehensive security architecture ensures that Scribilis maintains the highest standards of data protection, user privacy, and system security while providing an engaging educational experience for children and peace of mind for parents.
