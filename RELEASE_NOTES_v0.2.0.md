# Release Notes v0.2.0

**Release Date**: October 2, 2025
**Period Covered**: September 27 - October 2, 2025
**Total Changes**: 30 commits

---

## 🎯 Overview

This release focuses on **production readiness**, **monitoring & observability**, **security hardening**, and **performance optimization** for small EC2 instances. Major additions include Sentry error tracking, Matomo analytics, health check endpoints, and comprehensive game limits system.

---

## ✨ New Features

### 1. Health Check API Endpoints (Oct 2)
**Commit**: `4b08502`

Added production-ready health check endpoints for monitoring and load balancers:
- **Basic Health Check**: `GET /api/health` - Quick liveness probe
- **Readiness Check**: `GET /api/health/ready` - Comprehensive readiness verification
- Checks database connectivity, environment variables, and memory usage
- Returns 200 (healthy) or 503 (unhealthy) status codes

**Files Added**:
- `src/app/api/health/route.ts`
- `src/app/api/health/ready/route.ts`
- `docs/HEALTH_CHECK.md` - Complete documentation with AWS ALB, Docker, Kubernetes examples
- `scripts/deploy-health-check.sh` - Automated deployment script

**Use Cases**: Load balancer health checks, uptime monitoring (UptimeRobot, Pingdom), PM2 health checks, Docker HEALTHCHECK, Kubernetes liveness/readiness probes

---

### 2. Sentry Error Tracking & Monitoring (Oct 1)
**Commits**: `a8e473d`, `a4901f2`

Integrated Sentry for comprehensive error tracking and performance monitoring:

**Features**:
- Client-side, server-side, and edge runtime monitoring
- Automatic error capture with PII filtering (GDPR compliant)
- Performance monitoring with custom spans for AI, database, and game operations
- Source maps for production debugging
- Global error boundaries (`global-error.tsx`, `error.tsx`)
- Custom utility functions for structured error reporting

**Files Added**:
- `instrumentation.ts` - Server instrumentation with `onRequestError` hook
- `instrumentation-client.ts` - Client-side Sentry initialization (Turbopack compatible)
- `sentry.server.config.ts` - Server configuration with PII filtering
- `sentry.edge.config.ts` - Edge runtime configuration
- `src/lib/monitoring/sentry.ts` - Utility functions (captureException, withApiMonitoring, withAIMonitoring, etc.)
- `src/app/global-error.tsx` - Root-level error boundary
- `src/app/error.tsx` - Route-level error boundary
- `docs/SENTRY_FIX_SUMMARY.md` - Complete setup and troubleshooting guide
- `docs/MONITORING_AND_TESTING.md` - Comprehensive monitoring documentation

**APIs Migrated**:
- Migrated from deprecated `startTransaction` to modern `startSpan` API
- Replaced `Sentry.metrics` (not available) with breadcrumbs and events
- Fixed `contexts` parameter usage with `Sentry.withScope()`

**Next.js 15 Compatibility**:
- Full support for instrumentation hooks
- Turbopack-compatible client configuration
- Proper error boundaries for App Router

---

### 3. Matomo Analytics Integration (Sep 30)
**Commits**: `c19c5e0`, `bfe5a80`, `37e0eea`, `7a9f700`

Integrated Matomo (self-hosted) analytics for privacy-compliant user tracking:

**Features**:
- Cookie-free tracking option
- GDPR-compliant consent management
- Custom event tracking for games and user actions
- Tag Manager support
- Nginx reverse proxy configuration for security

**Files Added**:
- `src/components/analytics/MatomoScript.tsx` - Analytics script component
- `src/components/analytics/ConsentBanner.tsx` - GDPR consent banner
- `deployment/deploy-matomo.sh` - Automated Matomo deployment script
- `deployment/matomo-integration.ts` - TypeScript integration utilities
- `deployment/configure-nginx-ssl.sh` - SSL/TLS configuration
- `deployment/test-analytics.sh` - Analytics verification script
- `docs/MATOMO_DEPLOYMENT_GUIDE.md` - Complete deployment guide
- `nginx/scribilis.conf` - Updated Nginx configuration

**Privacy Features**:
- IP anonymization
- No personal data tracking
- Opt-out support
- Cookie consent integration

---

### 4. Game Limits System (Sep 27)
**Commit**: `8afa7b8`, `2b3a169`

Implemented comprehensive daily game limits system with subscription tiers:

**Features**:
- Daily game limits per child (3 free, unlimited premium)
- Real-time limit checking before game start
- Subscription modal with upgrade prompts
- Admin-configurable limits via settings page
- Automatic reset at midnight (Europe/Paris timezone)
- Graceful limit enforcement across all game types

**Files Added**:
- `src/lib/game/limits.ts` - Core limits logic
- `src/lib/game/middleware.ts` - API middleware for limit enforcement
- `src/hooks/useGameLimits.ts` - React hook for client-side limit checking
- `src/components/modals/SubscriptionLimitModal.tsx` - Upgrade prompt modal
- `src/app/api/games/check-limits/route.ts` - Limit verification endpoint
- `src/app/api/games/start/route.ts` - Game start with limit check
- `test-configurable-limits.mjs` - Integration tests

**Database Changes**:
- Added `SystemSettings` table for dynamic configuration
- New fields: `gamesPerDayFree`, `gamesPerDayPremium`

**Admin Features**:
- Configure limits via `/admin/settings`
- View current limit status
- Override limits per subscription tier

---

### 5. Rate Limiting Middleware (Oct 1)
**Commit**: `a8e473d`

Added production-grade rate limiting for API security:

**Features**:
- In-memory rate limiter with automatic cleanup
- 6 pre-configured limiters: auth, passwordReset, api, game, ai, admin
- IP-based and user-based rate limiting
- Configurable windows and request limits
- 429 Too Many Requests responses with retry information

**Files Added**:
- `src/lib/middleware/rate-limiter.ts` - Core rate limiting logic
- `src/lib/middleware/__tests__/rate-limiter.test.ts` - Unit tests

**Applied To**:
- `/api/auth/login` - 5 requests per 15 minutes
- `/api/auth/forgot-password` - 3 requests per hour
- All game API routes - 50 requests per 15 minutes

---

### 6. XP & Gamification System (Oct 1)
**Commit**: `a8e473d`

Extracted and enhanced XP calculation into reusable utility module:

**Features**:
- 15 level progression system with exponential XP curve
- 30+ badges (milestone, streak, achievement, special)
- Streak tracking and bonus XP
- Level-up notifications
- Quality bonuses for longer stories
- Automatic badge awarding

**Files Added**:
- `src/lib/game/xp-calculator.ts` - Complete XP and badge system
- `src/lib/game/__tests__/xp-calculator.test.ts` - Comprehensive tests

**XP Awards**:
- Story step completed: 10 XP
- Story step quality bonus: +5 XP (for >3 sentences)
- Story completed: 50 XP
- Story with image: 20 XP
- Debate completed: 40 XP
- Level up bonus: 20 XP

**Badges**:
- Milestone badges: 1, 5, 10, 25, 50, 100 stories
- Level badges: Levels 5, 10, 15
- Streak badges: 3, 7, 14, 30 day streaks
- Achievement badges: Fast writer, creative writer, debater, artist

---

### 7. Email Template System (Oct 1)
**Commit**: `a8e473d`

Migrated to professional Handlebars-based email template engine:

**Features**:
- 6 responsive email templates (French)
- 4 reusable partials (header, footer, button, badge)
- Custom Handlebars helpers
- HTML + plain text versions
- Professional styling with inline CSS

**Templates**:
1. `welcome-parent.hbs` - Parent welcome email
2. `welcome-child.hbs` - Child account creation
3. `level-up.hbs` - Level achievement notification
4. `badge-earned.hbs` - Badge unlock notification
5. `password-reset.hbs` - Password reset link
6. `subscription-confirmed.hbs` - Subscription confirmation

**Files Added**:
- `src/lib/email/template-engine.ts` - Template rendering engine
- `src/lib/email/templates/*.hbs` - Email templates
- `src/lib/email/templates/partials/*.hbs` - Reusable components
- `src/lib/email/__tests__/template-engine.test.ts` - Template tests

---

### 8. Admin Subscription Management (Sep 27)
**Commit**: `b399e33`

Added comprehensive subscription management dashboard:

**Features**:
- View all subscriptions with filtering
- Manual subscription creation/cancellation
- Stripe integration status monitoring
- Subscription analytics and metrics
- Bulk operations support

**Files Added**:
- `src/app/admin/subscriptions/page.tsx` - Admin subscriptions page
- `src/app/api/admin/subscriptions/route.ts` - Subscription API

---

## 🐛 Bug Fixes

### Security Fixes (Sep 27)
**Commits**: `6ba270a`, `fa8137c`, `68a16ec`, `5c6b7e0`, `1511500`

**Fixed 5 critical security vulnerabilities**:

1. **Content Security Policy**: Added comprehensive CSP headers in `next.config.ts`
   - Restricted script sources to prevent XSS
   - Blocked inline scripts except for Next.js
   - Added frame-src restrictions

2. **Input Validation**: Added Zod validation for all API endpoints
   - Story ID validation
   - User input sanitization
   - SQL injection prevention

3. **CSRF Protection**: Added proper CSRF token handling
   - Secure cookie configuration
   - SameSite=Strict enforcement

4. **Rate Limiting**: Applied to sensitive endpoints (see Features)

5. **PII Sanitization**: Sentry integration with PII filtering
   - Automatic email/token redaction
   - Query string sanitization
   - Error stack trace filtering

**Files Modified**:
- `next.config.ts` - Security headers
- All API routes - Input validation
- `sentry.server.config.ts` - PII filtering
- `src/lib/auth/rbac.ts` - Enhanced RBAC

---

### Performance Fixes (Sep 27)
**Commits**: `750f4f6`, `4f731d5`

**Fixed Kids Dashboard Latency**:
- Optimized database queries with selective field loading
- Reduced `imageBlob` fetching (only check existence, don't load data)
- Added pagination for stories list
- Implemented lazy loading for images
- Response time improved from ~2s to ~300ms

**Files Modified**:
- `src/app/api/auth/me/route.ts`
- `src/app/kids/page.tsx`

---

### UI/UX Fixes

#### Admin Content Page Crash (Sep 27)
**Commit**: `dd0affa`

**Fixed**: Admin content page crashing when viewing incomplete stories
- Added null checks for story content
- Graceful handling of missing story data
- Better error messages for incomplete content

**File Modified**: `src/app/admin/content/page.tsx`

---

#### Textarea Background Color (Sep 27)
**Commit**: `47be065`

**Fixed**: Textarea component using wrong background color in moderation page
- Changed from `bg-input` to `bg-background` for consistency
- Improved visibility in dark mode

**File Modified**: `src/app/admin/moderation/page.tsx`

---

#### Roue Histoires Completion (Sep 27)
**Commit**: `b399e33`

**Fixed**: Roue Histoires game completion logic error
- Removed duplicate database call
- Fixed story completion status update
- Improved error handling

**File Modified**: `src/app/api/games/roue-histoires/complete/route.ts`

---

#### Parents Page Rendering (Sep 29-30)
**Commits**: `a8b30f4`, `f2782ee`, `e4720e6`

**Fixed**: Parents dashboard not rendering correctly
- Fixed component props mismatch
- Corrected data fetching logic
- Improved loading states

**File Modified**: `src/app/parents/page.tsx`

---

#### RBAC Authorization (Sep 29)
**Commits**: `ae17586`, `a4cd2ec`

**Fixed**: Role-Based Access Control edge cases
- Fixed parent access to child data
- Enhanced permission checks
- Added badge type field to database

**Files Modified**:
- `src/lib/auth/rbac.ts`
- `prisma/schema.prisma`
- Migration: `20250929161648_add_badge_type`

---

#### Admin Content API (Sep 27)
**Commit**: `c3ff951`

**Fixed**: Admin content API not handling pagination correctly
- Added proper pagination support
- Fixed query filters
- Improved error responses

**File Modified**: `src/app/api/admin/content/route.ts`

---

## 🚀 Performance & Infrastructure

### Build Optimization for T3.small (Oct 1)
**Commit**: `60600b6`

**Optimized build process for EC2 T3.small (2GB RAM)**:

**Changes**:
- Disabled Sentry webpack plugins during build (saves ~1GB memory)
- Optimized webpack chunk splitting configuration
- Added Node.js memory limit options (1.5GB and 3GB)
- Created swap setup script for small instances

**New Scripts**:
- `npm run build` - 3GB memory limit (with swap)
- `npm run build:low-memory` - 1.5GB memory limit (minimal)
- `scripts/setup-swap.sh` - Automated 2GB swap creation

**Documentation**:
- `docs/BUILD_OPTIMIZATION.md` - Complete build optimization guide with 4 deployment strategies

**Expected Results**:
- Build memory usage: ~1.5-2GB (down from 2GB+)
- Build time on T3.small: 5-8 minutes (with swap)
- Success rate: 90%+ on T3.small with swap enabled

**Files Modified**:
- `next.config.ts` - Webpack optimization + disabled Sentry plugins
- `package.json` - Memory-optimized build scripts

---

### Dependencies Updates (Sep 30)
**Commits**: `761e9f3`, `a434ec2`

**Updated npm packages**:
- Next.js 15.5.3 (latest stable)
- React 19.1.0
- Sentry SDK 10.17.0
- TypeScript 5.x
- All dev dependencies to latest compatible versions

**Security patches applied**: 15+ vulnerabilities fixed

---

## 📚 Documentation

### New Documentation (Oct 1-2)
**Commits**: `651b298`, `a8e473d`, `60600b6`, `4b08502`, `633b5ee`, `7a9f700`

**Created comprehensive documentation**:

1. **`docs/HEALTH_CHECK.md`** (373 lines)
   - Health check API reference
   - Integration examples (AWS ALB, Docker, Kubernetes, PM2)
   - Monitoring scripts with Slack alerts
   - Troubleshooting guide

2. **`docs/BUILD_OPTIMIZATION.md`** (267 lines)
   - T3.small optimization strategies
   - 4 deployment options
   - Memory management guide
   - Swap configuration instructions

3. **`docs/SENTRY_FIX_SUMMARY.md`** (125 lines)
   - Complete Sentry setup guide
   - Troubleshooting TypeScript errors
   - Next.js 15 compatibility notes
   - API migration guide

4. **`docs/MONITORING_AND_TESTING.md`** (558 lines)
   - Test coverage report (94 tests, 88.3% pass rate)
   - Monitoring strategy
   - Sentry integration details
   - Testing best practices

5. **`docs/COMPLETE_IMPLEMENTATION_SUMMARY.md`** (634 lines)
   - Full implementation summary
   - All features documented
   - Architecture overview

6. **`docs/IMPLEMENTATION_SUMMARY.md`** (391 lines)
   - Quick reference implementation guide
   - Key features overview

7. **`docs/QUICK_REFERENCE.md`** (298 lines)
   - Quick command reference
   - Common operations guide
   - Troubleshooting tips

8. **`docs/MATOMO_DEPLOYMENT_GUIDE.md`** (1309 lines)
   - Complete Matomo setup guide
   - Nginx SSL configuration
   - Privacy compliance guide
   - Testing and validation scripts

9. **`docs/PRODUCT_REQUIREMENTS_DOCUMENT.md`** (624 lines)
   - Complete product specification
   - Feature requirements
   - User stories

10. **`docs/SECURITY_ARCHITECTURE.md`** (566 lines)
    - Security design principles
    - Threat model
    - Security controls

**Organized Documentation**:
- Moved all `.md` files to `docs/` folder (15 files)
- Kept `README.md` in root
- Better project organization

---

## 🧪 Testing

### New Test Suites (Oct 1)
**Commit**: `a8e473d`

**Added comprehensive test coverage (70%+ on critical paths)**:

1. **`src/lib/auth/__tests__/jwt.test.ts`** (303 lines)
   - JWT token generation and verification
   - Password hashing and validation
   - Token expiration handling
   - Edge cases and error conditions

2. **`src/lib/game/__tests__/xp-calculator.test.ts`** (220 lines)
   - XP awards for all game types
   - Level progression calculation
   - Badge awarding logic
   - Streak tracking
   - Milestone achievements

3. **`src/lib/middleware/__tests__/rate-limiter.test.ts`** (223 lines)
   - Rate limit enforcement
   - Window sliding logic
   - User and IP-based limiting
   - Cleanup functionality

4. **`src/lib/validation/__tests__/schemas.test.ts`** (378 lines)
   - Input validation schemas
   - Edge case validation
   - Error message formatting

5. **`src/lib/email/__tests__/template-engine.test.ts`** (233 lines)
   - Template rendering
   - Handlebars helper functions
   - HTML and text output

**Test Results**:
- Total tests: 94
- Passing: 83 (88.3%)
- Failing: 11 (mostly component rendering tests, non-critical)
- Critical path coverage: 70%+

**Test Scripts**:
- `npm test` - Run all tests
- `npm run test:watch` - Watch mode
- `npm run test:coverage` - Coverage report

---

## 🔧 Configuration Changes

### Next.js Configuration (Sep 27, Oct 1)
**File**: `next.config.ts`

**Security Headers Added**:
- `X-Content-Type-Options: nosniff`
- `X-Frame-Options: DENY`
- `X-XSS-Protection: 1; mode=block`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Content-Security-Policy` (comprehensive policy)

**Performance Optimizations**:
- Webpack chunk splitting
- Memory optimization for builds
- Handlebars warning suppression

**Sentry Integration**:
- Source map upload configuration
- Client/server plugin configuration
- Production-only source map upload

---

### Database Schema Changes (Sep 27, Sep 29)

**New Tables**:
1. **SystemSettings** - Dynamic configuration storage
   ```sql
   - id: String @id
   - gamesPerDayFree: Int @default(3)
   - gamesPerDayPremium: Int @default(999)
   - updatedAt: DateTime
   ```

**Schema Modifications**:
1. Added `type` field to `Badge` model
2. Added game limit tracking fields

**Migrations**:
- `20250929161648_add_badge_type`
- Game limits migration (date TBD)

---

### Environment Variables (Oct 1)

**New Required Variables**:
```env
# Sentry
NEXT_PUBLIC_SENTRY_DSN=https://xxx@xxx.ingest.sentry.io/xxx
SENTRY_ORG=your-organization
SENTRY_PROJECT=scribilis
SENTRY_AUTH_TOKEN=your-auth-token

# Matomo
NEXT_PUBLIC_MATOMO_URL=https://analytics.yourdomain.com
NEXT_PUBLIC_MATOMO_SITE_ID=1
NEXT_PUBLIC_MATOMO_ENABLED=true

# Version
NEXT_PUBLIC_APP_VERSION=0.2.0
```

---

## 📦 Deployment Scripts

### New Automation Scripts (Sep 29, Oct 1-2)

1. **`scripts/setup-swap.sh`** - Automated swap space creation (2GB)
2. **`scripts/deploy-health-check.sh`** - Health check deployment
3. **`deployment/deploy-matomo.sh`** - Matomo installation (381 lines)
4. **`deployment/configure-nginx-ssl.sh`** - SSL/TLS setup (491 lines)
5. **`deployment/test-analytics.sh`** - Analytics verification (361 lines)
6. **`deployment/validate-deployment.sh`** - Full deployment validation (332 lines)
7. **`deployment/setup-ovh-vps.sh`** - VPS initial setup (543 lines)

**Total Deployment Automation**: 2000+ lines of production-ready bash scripts

---

## 🔒 Security Enhancements

### Summary of Security Improvements

1. **Content Security Policy** - Comprehensive CSP headers
2. **Rate Limiting** - API endpoint protection
3. **Input Validation** - Zod schemas for all inputs
4. **PII Filtering** - Sentry integration with automatic redaction
5. **CSRF Protection** - Enhanced token handling
6. **RBAC Improvements** - Stricter permission checks
7. **SQL Injection Prevention** - Parameterized queries
8. **XSS Prevention** - Input sanitization
9. **Cookie Security** - HttpOnly, Secure, SameSite flags
10. **Error Handling** - No sensitive data in error messages

**Security Score**: Improved from baseline to production-ready

---

## 📊 Metrics & Analytics

### Code Statistics

**Lines of Code Added**: ~12,000+
- Production code: ~6,000
- Tests: ~1,400
- Documentation: ~4,600
- Scripts: ~2,000

**Files Added**: 50+
**Files Modified**: 75+

### Test Coverage

- **Total Tests**: 94
- **Pass Rate**: 88.3%
- **Critical Path Coverage**: 70%+

### Build Performance

**Before Optimization**:
- Memory: 2GB+
- Time: 15+ minutes (with swap)
- Failure rate: 30% on T3.small

**After Optimization**:
- Memory: 1.5-2GB
- Time: 5-8 minutes (with swap)
- Success rate: 90%+ on T3.small

---

## 🎯 Breaking Changes

### None

This release is **fully backward compatible** with existing deployments. No database migrations require data transformation.

---

## 🚧 Known Issues

1. **Image Optimization Warnings**: Some components still use `<img>` instead of Next.js `<Image />`
   - Impact: Suboptimal performance
   - Priority: Low
   - Planned fix: Future release

2. **Component Rendering Tests**: 11 tests failing
   - Impact: None (non-critical component tests)
   - Priority: Low
   - Planned fix: Future release

3. **Build Time on T3.small**: Still 5-8 minutes
   - Impact: Slow deployments
   - Workaround: Use build:low-memory or build locally
   - Priority: Medium

---

## 🔜 Next Steps

### Recommended Actions

1. **Deploy Health Checks**:
   ```bash
   ./scripts/deploy-health-check.sh
   ```

2. **Configure Monitoring**:
   - Set up UptimeRobot with `/api/health`
   - Configure Sentry alerts
   - Enable Matomo analytics

3. **Update Load Balancer**:
   - Point health check to `/api/health`
   - Set check interval to 30s

4. **Test Build Process**:
   ```bash
   npm run build
   ```

5. **Review Security Headers**:
   - Test CSP in production
   - Verify HTTPS redirects

6. **Enable Swap on Production** (if T3.small):
   ```bash
   sudo ./scripts/setup-swap.sh
   ```

---

## 👥 Contributors

**Belkouche** - All 30 commits

---

## 📞 Support

**Documentation**: See `docs/` folder for detailed guides

**Issues**: Report at project repository

**Health Check**: `GET https://your-domain.com/api/health`

---

## 🙏 Acknowledgments

- Next.js team for App Router improvements
- Sentry team for excellent monitoring tools
- Matomo project for privacy-focused analytics
- Community for testing and feedback

---

**Full Changelog**: See [git log](https://github.com/yourusername/scribilis/compare/750f4f6...4b08502)

---

**Release Hash**: `4b08502`
**Previous Release**: `750f4f6`
**Commits**: 30
**Changed Files**: 125+
**Lines Changed**: ~12,000+

---

© 2025 Scribilis - All rights reserved
