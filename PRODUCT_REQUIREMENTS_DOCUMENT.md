# Scribilis - Product Requirements Document (PRD)

## 1. Product Overview

**Product Name**: Scribilis
**Version**: 1.0
**Target Market**: French-speaking families with children aged 7-14 years
**Product Type**: Educational SaaS Platform

### Vision Statement
Scribilis empowers children to develop their writing and argumentation skills through engaging, AI-powered interactive storytelling games in a safe, parent-supervised environment.

### Core Value Proposition
- **For Children**: Fun, gamified learning experience that builds writing confidence and creativity
- **For Parents**: Safe educational platform with progress monitoring and parental controls
- **For Educators**: Structured skill development with measurable progress tracking

## 2. Target Audience

### Primary Users

#### Children (Ages 7-14)
- **Demographics**: French-speaking students in elementary and middle school
- **Goals**: Have fun while learning, express creativity, build writing skills
- **Pain Points**: Writing can be intimidating, lack of engaging practice opportunities
- **Tech Comfort**: High familiarity with digital games and interfaces

#### Parents (Ages 30-50)
- **Demographics**: Education-conscious parents seeking quality learning tools
- **Goals**: Support child's academic development, monitor progress, ensure online safety
- **Pain Points**: Screen time concerns, educational content quality, cost of multiple subscriptions
- **Tech Comfort**: Moderate to high, expect intuitive interfaces

### Secondary Users

#### Educators & Tutors
- **Use Case**: Supplementary tool for writing instruction
- **Goals**: Track student progress, assign structured writing activities
- **Integration**: Potential future feature for classroom management

## 3. Core Features & Functionality

### 3.1 Game Mechanics

#### A. Parcours à Histoire (Story Journey)
**Target Age**: 7-14 years | **Duration**: 15-20 minutes

- **Core Mechanic**: Step-by-step story creation with AI guidance
- **Features**:
  - Dice-based prompt generation (1-6 options per step)
  - Progressive narrative building (10 steps max)
  - Real-time AI feedback and suggestions
  - Story completion validation
  - Save and share completed stories

- **Educational Goals**:
  - Narrative structure understanding
  - Creative thinking development
  - Writing flow and coherence
  - Vocabulary expansion

#### B. Roue à Histoires (Story Wheel)
**Target Age**: 7-14 years | **Duration**: 20-30 minutes

- **Core Mechanic**: Random element generator for creative writing
- **Features**:
  - Interactive spinning wheel with character/setting/object prompts
  - Free-form story composition
  - Minimum sentence requirements (configurable: 5-20 sentences)
  - AI-powered feedback system (interval: 30-300 seconds)
  - Story portfolio building

- **Educational Goals**:
  - Creative constraint handling
  - Character development skills
  - Plot construction
  - Extended writing practice

#### C. L'Argumentarium (Debate Arena)
**Target Age**: 9-14 years | **Duration**: 10-15 minutes

- **Core Mechanic**: Structured debate practice with AI opponent
- **Features**:
  - Age-appropriate topic selection
  - Turn-based argument construction
  - AI-generated counter-arguments
  - Argument quality scoring
  - Debate session summaries

- **Educational Goals**:
  - Critical thinking development
  - Argument structure (claim, evidence, reasoning)
  - Perspective-taking abilities
  - Logical reasoning skills

### 3.2 Progression System

#### XP & Leveling
- **XP Awards**: Variable points per completed activity
- **Level System**: 1-10+ levels, 100 XP per level advancement
- **Progress Tracking**: Visual progress bars and achievement unlocks
- **Skill Metrics**: Writing fluency, creativity score, argument strength

#### Daily Limits & Engagement
- **Free Users**: Configurable daily game limit (1-10 games, default: 5)
- **Subscribed Users**: Unlimited gameplay
- **Session Management**: Automatic save states, resume capabilities
- **Healthy Usage**: Built-in break reminders, session time tracking

### 3.3 Content Management

#### Story Portfolio
- **Personal Library**: Save all completed stories and debates
- **Sharing Controls**: Parent-approved sharing with family
- **Progress Visualization**: Writing improvement over time
- **Export Options**: PDF generation for offline reading

#### AI-Powered Features
- **Multi-Provider Support**: Choose from Anthropic Claude, OpenAI GPT, or Mistral AI
- **Content Generation**: Dynamic prompts and scenarios across all providers
- **Real-time Feedback**: Writing suggestions and improvements with AI assistance
- **Safety Moderation**: Automated inappropriate content detection using AI models
- **Adaptive Difficulty**: Adjusts to child's skill level based on AI analysis
- **Cost Optimization**: Provider selection based on cost-effectiveness and quality
- **European Data Compliance**: Mistral AI option for GDPR-focused deployments

## 4. User Authentication & Roles

### 4.1 User Types

#### Child Account
- **Login Method**: Username + password
- **Profile**: Avatar, grade level, interests
- **Permissions**: Game access, story creation, limited settings
- **Safety**: Restricted communication, content filtering

#### Parent Account
- **Login Method**: Email + password
- **Profile**: Family management, billing information
- **Permissions**: Full child monitoring, subscription management
- **Features**: Progress reports, content review, sharing controls

#### Admin Account
- **Login Method**: Email + password (elevated privileges)
- **Permissions**: Platform configuration, user management, analytics
- **Tools**: Content moderation, system monitoring, KPI tracking

### 4.2 Account Relationships

#### Parent-Child Sharing
- **Multi-Child Support**: Parents can manage multiple children
- **Shared Access**: Secondary parents with configurable permissions
- **Permission Levels**:
  - View progress only
  - Modify child profile
  - Manage sharing settings
  - Delete account access

## 5. Admin Dashboard & Analytics

### 5.1 Key Performance Indicators

#### Business Metrics
- **Monthly Recurring Revenue (MRR)**: Subscription revenue tracking
- **User Growth**: Registration and activation rates
- **Churn Analysis**: Subscription cancellation patterns
- **Customer Lifetime Value**: Revenue per user calculations

#### Engagement Metrics
- **Daily/Monthly Active Users**: Platform usage patterns
- **Session Duration**: Average time spent per game type
- **Completion Rates**: Game finish percentage by age group
- **Content Creation**: Stories and debates generated daily

#### Educational Metrics
- **Skill Progression**: Writing improvement measurements
- **Achievement Unlocks**: Badge and level advancement rates
- **Parent Engagement**: Dashboard usage and feedback patterns

### 5.2 Content Moderation

#### Automated Systems
- **AI Content Review**: Real-time inappropriate content detection
- **Configurable Moderation Levels**:
  - Lenient: Creative fantasy content allowed
  - Moderate: Balanced creativity and safety
  - Strict: Educational content only
- **Alert System**: Automatic flagging for manual review

#### Manual Review Process
- **Admin Queue**: Content requiring human review
- **Parent Reporting**: Community-driven safety monitoring
- **Response Time**: 24-hour review commitment
- **Appeals Process**: Content restoration procedures

### 5.3 System Configuration

#### AI Provider Management
- **Multi-Provider Configuration**: Switch between Anthropic Claude, OpenAI, and Mistral AI per game type
- **Model Selection**: Choose specific models within each provider (GPT-4, Claude-3.5-Sonnet, Mistral-Large, etc.)
- **Cost Optimization**: Track usage and costs across all providers with automatic recommendations
- **Provider Health Monitoring**: Real-time status checking and automatic failover
- **Custom Parameters**: Temperature, max tokens, and system prompts per game type

#### Game Settings (Configurable)
- **Feedback Interval**: 30-300 seconds for Story Wheel
- **Daily Limits**: 1-20 stories per day maximum
- **Completion Requirements**: 5-20 minimum sentences
- **Free User Limits**: 1-10 daily games for non-subscribers (now configurable)

## 6. Technical Architecture & Infrastructure

### 6.1 Complete Technology Stack

#### Frontend Framework & Libraries
- **Core Framework**: Next.js 15.5.3 with React 19.1.0
- **Language**: TypeScript 5+ for type safety
- **Build Tool**: Turbopack for faster development builds
- **Styling Framework**: Tailwind CSS 4 with custom child-friendly themes
- **UI Components**:
  - shadcn/ui with Radix UI primitives
  - Lucide React for icons
  - Motion (Framer Motion) for animations
- **State Management**: React hooks, context, and React Hook Form
- **Charts & Visualization**: Recharts 3.2.1 for analytics dashboards

#### Backend Architecture
- **API Framework**: Next.js API Routes (serverless-ready)
- **Runtime**: Node.js 20+ LTS
- **Process Management**: PM2 for production deployments
- **Database**: PostgreSQL with Prisma ORM 6.16.2
- **Authentication**:
  - Custom JWT implementation with jsonwebtoken
  - NextAuth.js 4.24.11 for OAuth integration
  - bcryptjs for password hashing

#### Third-Party Service Integrations
- **AI Services**:
  - Anthropic Claude SDK 0.64.0 (primary provider)
  - OpenAI GPT API 5.21.0 (secondary provider)
  - Mistral AI SDK 1.10.0 (cost-effective European provider)
- **Payment Processing**: Stripe 18.5.0 (both client and server)
- **Email Services**: Nodemailer 6.10.1 for transactional emails
- **Validation**: Zod 3.25.76 for schema validation

#### Development & Testing Tools
- **Testing Framework**: Jest 30.1.3 with React Testing Library 16.3.0
- **Linting**: ESLint 9 with Next.js configuration
- **Type Checking**: Native TypeScript compiler
- **Database Tools**: Prisma Studio for database management

### 6.2 Hosting Infrastructure Requirements

#### Recommended Cloud Providers (Priority Order)

#### Option 1: AWS (Amazon Web Services) - Recommended
**Advantages**: Global CDN, extensive services, scalability, GDPR compliance

**Core Services Required**:
- **Compute**: EC2 t3.medium instances (2 vCPU, 4GB RAM) minimum
- **Database**: RDS PostgreSQL (db.t3.micro for staging, db.t3.small for production)
- **Storage**: S3 for file storage and backups
- **CDN**: CloudFront for global content delivery
- **Load Balancer**: Application Load Balancer (ALB)
- **DNS**: Route 53 for domain management
- **SSL**: AWS Certificate Manager for free SSL certificates
- **Monitoring**: CloudWatch for logs and metrics

**Estimated Monthly Cost**:
- Staging Environment: $50-75/month
- Production Environment: $150-300/month (scales with usage)

**Configuration**:
```
Production Instance: t3.medium (2 vCPU, 4GB RAM)
Database: db.t3.small (2 vCPU, 2GB RAM)
Storage: 100GB S3 + 20GB EBS
Bandwidth: 1TB/month included
```

#### Option 2: OVHcloud (European Focus) - Cost-Effective
**Advantages**: European data centers, GDPR native, competitive pricing, French support

**Core Services Required**:
- **Compute**: VPS SSD 2 (2 vCPU, 4GB RAM) - €8/month
- **Database**: Managed PostgreSQL - €25/month
- **Storage**: Object Storage - €0.01/GB
- **CDN**: OVHcloud CDN - €5/month
- **Load Balancer**: Load Balancer OVHcloud - €15/month
- **DNS**: Domain & DNS management included
- **SSL**: Let's Encrypt integration

**Estimated Monthly Cost**:
- Staging Environment: €25-35/month
- Production Environment: €75-120/month

**Configuration**:
```
Production VPS: 2 vCPU, 4GB RAM, 80GB SSD
Database: 2 vCPU, 4GB RAM, 20GB storage
CDN: European PoPs
Backup: Automated daily snapshots
```

#### Option 3: DigitalOcean - Developer-Friendly
**Advantages**: Simple pricing, good documentation, developer tools

**Core Services Required**:
- **Compute**: Droplets - $24/month (2 vCPU, 4GB)
- **Database**: Managed PostgreSQL - $15/month
- **Storage**: Spaces Object Storage - $5/month
- **CDN**: Spaces CDN included
- **Load Balancer**: $10/month
- **DNS**: Free DNS management

**Estimated Monthly Cost**:
- Staging Environment: $20-30/month
- Production Environment: $60-100/month

#### Option 4: Vercel + External Database - Serverless
**Advantages**: Zero-config Next.js deployment, global edge network

**Services Configuration**:
- **Hosting**: Vercel Pro Plan - $20/month
- **Database**: External PostgreSQL (PlanetScale, Supabase, or Neon)
- **File Storage**: Vercel Blob or external S3-compatible
- **CDN**: Included with Vercel Edge Network

**Limitations**:
- Serverless function limits (10-second timeout)
- Cold start latency for AI operations
- Limited server-side session management

### 6.3 Infrastructure Requirements by Environment

#### Development Environment
```yaml
Minimum Requirements:
  CPU: 2 cores
  RAM: 4GB
  Storage: 20GB SSD
  Database: Local PostgreSQL or Docker
  External Services: Development API keys
```

#### Staging Environment
```yaml
Server Specifications:
  CPU: 2 vCPU
  RAM: 4GB
  Storage: 40GB SSD
  Database: Managed PostgreSQL (2GB RAM, 20GB storage)
  Load Balancer: Not required
  CDN: Optional
  SSL: Let's Encrypt or provider SSL
  Backup: Daily automated backups
```

#### Production Environment
```yaml
Server Specifications:
  CPU: 4 vCPU (minimum 2 vCPU)
  RAM: 8GB (minimum 4GB)
  Storage: 100GB SSD
  Database: Managed PostgreSQL (4GB RAM, 50GB+ storage)
  Load Balancer: Required for high availability
  CDN: Required for global performance
  SSL: Wildcard SSL certificate
  Backup: 3x daily backups with 30-day retention
  Monitoring: Full logging and alerting
```

### 6.4 Performance & Scaling Requirements

#### Traffic Projections
```
Month 1-3: 100-500 concurrent users
Month 4-6: 500-1,500 concurrent users
Month 7-12: 1,500-5,000 concurrent users
Year 2+: 5,000+ concurrent users
```

#### Database Scaling Strategy
- **Phase 1**: Single PostgreSQL instance with connection pooling
- **Phase 2**: Read replicas for analytics and reporting
- **Phase 3**: Database sharding by tenant/family
- **Phase 4**: Separate databases for different services

#### Application Scaling
- **Horizontal Scaling**: PM2 cluster mode or container orchestration
- **Caching Strategy**: Redis for session storage and API caching
- **CDN Strategy**: Static assets via CloudFront/CDN
- **API Rate Limiting**: Implement rate limiting per user type

### 6.5 Security & Compliance Infrastructure

#### SSL/TLS Requirements
- **Minimum**: TLS 1.2
- **Recommended**: TLS 1.3
- **Certificate Type**: Wildcard SSL for all subdomains
- **HSTS**: HTTP Strict Transport Security enabled

#### Backup & Disaster Recovery
- **Database Backups**: Automated daily backups with 30-day retention
- **Application Backups**: Daily server snapshots
- **Geographic Redundancy**: Cross-region backup storage
- **Recovery Time Objective (RTO)**: 4 hours maximum
- **Recovery Point Objective (RPO)**: 1 hour maximum

#### Monitoring & Logging
```yaml
Required Monitoring:
  - Application performance (response times, errors)
  - Database performance (query times, connections)
  - Server resources (CPU, RAM, disk usage)
  - User activity and security events
  - AI service usage and costs
  - Payment transaction monitoring

Log Retention:
  - Application logs: 90 days
  - Security logs: 365 days
  - Access logs: 30 days
  - Error logs: 180 days
```

### 6.6 DevOps & Deployment Pipeline

#### Recommended Deployment Strategy
```yaml
Development Pipeline:
  1. Local development with hot reload
  2. Git push triggers automated testing
  3. Staging deployment for QA testing
  4. Production deployment with zero downtime
  5. Automated rollback capability

Tools Required:
  - Git repository (GitHub/GitLab)
  - CI/CD pipeline (GitHub Actions/GitLab CI)
  - Container registry (optional)
  - Infrastructure as Code (Terraform/CDK)
  - Monitoring and alerting system
```

#### Environment Variables & Configuration
```env
# Essential Environment Variables
NODE_ENV=production
DATABASE_URL=postgresql://...
NEXTAUTH_SECRET=...
NEXTAUTH_URL=https://scribilis.com
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...
ANTHROPIC_API_KEY=...
OPENAI_API_KEY=...
SMTP_HOST=...
SMTP_PORT=587
SMTP_USER=...
SMTP_PASSWORD=...
```

### 6.7 Cost Optimization Strategy

#### Initial Phase (0-1000 users)
- **Budget**: €100-200/month total infrastructure
- **Focus**: Single server deployment with managed database
- **Optimization**: Reserved instances, right-sizing resources

#### Growth Phase (1000-5000 users)
- **Budget**: €300-600/month total infrastructure
- **Focus**: Load balancing, CDN, monitoring
- **Optimization**: Auto-scaling, caching implementation

#### Scale Phase (5000+ users)
- **Budget**: €600-1500/month total infrastructure
- **Focus**: Multi-region, high availability, advanced monitoring
- **Optimization**: Container orchestration, microservices migration

### 6.2 Security & Compliance

#### Data Protection
- **GDPR Readiness**: Data portability and deletion procedures
- **Child Privacy**: COPPA-compliant data handling
- **Encryption**: Data at rest and in transit protection
- **Access Controls**: Role-based permissions throughout

#### Content Safety
- **Input Validation**: SQL injection and XSS prevention
- **Rate Limiting**: API abuse protection
- **Content Filtering**: Multi-layer inappropriate content detection
- **Audit Logging**: Complete action tracking for compliance

## 7. Subscription Model & Monetization

### 7.1 Pricing Strategy

#### Free Tier
- **Daily Limit**: 5 games per day (configurable)
- **Features**: All game types, basic progress tracking
- **Duration**: Unlimited with usage restrictions
- **Upgrade Path**: Clear value proposition for premium features

#### Premium Family Plan
- **Price**: €9.99/month or €99.99/year (estimated)
- **Features**: Unlimited gameplay, advanced analytics, priority support
- **Family Size**: Up to 4 children per subscription
- **Trial Period**: 14-day free trial

### 7.2 Revenue Optimization

#### Conversion Strategies
- **Freemium Model**: Generous free tier with clear upgrade incentives
- **Family Value**: Multi-child support encourages family subscriptions
- **Educational Positioning**: Premium features for serious learners
- **Parent Dashboard**: Detailed progress reports drive subscription value

#### Cost Management & AI Provider Strategy
- **Multi-Provider Cost Optimization**: Automatic selection of cost-effective providers per use case
- **Real-time Cost Tracking**: Monitor usage and costs across Anthropic, OpenAI, and Mistral AI
- **Provider Performance Comparison**: Quality vs cost analysis with automatic recommendations
- **European Market Advantage**: Mistral AI for GDPR compliance and competitive European pricing
- **Scaling Strategy**: Efficient resource allocation and provider load balancing
- **Feature Gating**: Strategic limitation of advanced features based on subscription tier

## 8. Development Roadmap

### 8.1 Phase 1: Core Platform (Current)
- ✅ User authentication and role management
- ✅ Three core games with multi-provider AI integration
- ✅ Multi-AI provider support (Anthropic Claude, OpenAI, Mistral AI)
- ✅ Configurable daily game limits for free users
- ✅ Basic parent dashboard and child progress tracking
- ✅ Subscription management with Stripe
- ✅ Advanced content moderation system
- ✅ Comprehensive admin dashboard with AI cost analytics

### 8.2 Phase 2: Enhanced Features (Next 3 months)
- Advanced analytics and reporting
- Mobile-responsive design optimization
- Additional game types and variations
- Enhanced AI personalization
- Improved parent notification system
- Performance optimization and scaling

### 8.3 Phase 3: Platform Expansion (3-6 months)
- Multi-language support (English, Spanish)
- Educator dashboard and classroom features
- Advanced gamification (leaderboards, competitions)
- Social features (family sharing, achievements)
- Mobile app development
- API for third-party integrations

### 8.4 Phase 4: Market Leadership (6+ months)
- AI tutoring and personalized learning paths
- Content marketplace for user-generated prompts
- Integration with school curriculum standards
- Advanced analytics and machine learning insights
- White-label solutions for educational institutions
- International market expansion

## 9. Success Metrics & KPIs

### 9.1 User Acquisition
- **Target**: 1,000 registered families in first 6 months
- **Activation Rate**: >80% complete onboarding process
- **Viral Coefficient**: >0.2 new users per existing user
- **Cost per Acquisition**: <€25 per family

### 9.2 Engagement & Retention
- **Daily Active Users**: >40% of registered children
- **Session Duration**: Average 15+ minutes per session
- **Weekly Retention**: >60% return within 7 days
- **Monthly Retention**: >40% active after 30 days

### 9.3 Educational Impact
- **Skill Progression**: Measurable writing improvement within 30 days
- **Completion Rates**: >70% finish rate for started games
- **Parent Satisfaction**: >4.5/5 stars in educational value surveys
- **Content Quality**: <1% inappropriate content detection rate

### 9.4 Business Performance
- **Conversion Rate**: >15% free to paid subscription
- **Churn Rate**: <5% monthly subscription cancellation
- **Customer Lifetime Value**: >€120 per family
- **Monthly Recurring Revenue**: €10,000+ within 12 months

## 10. Risk Assessment & Mitigation

### 10.1 Technical Risks
- **AI Cost Scaling**: Monitor usage patterns, implement efficient caching
- **Database Performance**: Optimize queries, implement read replicas
- **Security Breaches**: Regular audits, penetration testing, incident response
- **Platform Reliability**: 99.9% uptime target, monitoring and alerting

### 10.2 Business Risks
- **Market Competition**: Focus on unique educational value proposition
- **Regulatory Changes**: Stay current with child privacy regulations
- **Economic Downturn**: Offer flexible pricing and value demonstration
- **User Safety Incidents**: Robust moderation and rapid response protocols

### 10.3 Product Risks
- **User Engagement**: Continuous A/B testing and feature optimization
- **Educational Effectiveness**: Partner with educators for validation
- **Parent Concerns**: Transparent communication and safety features
- **Technical Debt**: Regular refactoring and code quality maintenance

## 11. Conclusion

Scribilis represents a comprehensive educational platform that addresses the growing need for engaging, safe digital learning tools for children. By combining gamification, multi-provider AI assistance, and parental oversight, the platform creates a unique value proposition in the educational technology market.

### Competitive Advantages

**Multi-Provider AI Strategy**: The integration of Anthropic Claude, OpenAI, and Mistral AI provides unprecedented flexibility, cost optimization, and reliability. This approach ensures:
- **Cost Efficiency**: Mistral AI offers competitive European pricing
- **Quality Assurance**: Multiple providers ensure consistent service availability
- **Compliance Flexibility**: European providers like Mistral AI for GDPR-sensitive deployments
- **Future-Proofing**: Protection against single-provider dependencies

**Technical Excellence**: The robust architecture supports configurable game limits, real-time AI cost tracking, and seamless provider switching, setting Scribilis apart from single-provider competitors.

The phased development approach ensures rapid market entry while building towards a scalable, feature-rich platform that can compete with established educational technology providers. Success will be measured not only through traditional business metrics but also through demonstrable educational impact and user satisfaction.

The strong technical foundation, comprehensive safety features, multi-provider AI strategy, and clear monetization strategy position Scribilis for sustainable growth in the expanding EdTech market, with particular strength in the underserved French-language educational content space and European data compliance requirements.