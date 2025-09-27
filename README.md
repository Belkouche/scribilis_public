# Scribilis - Interactive Storytelling Platform for Children

Scribilis is a modern web application that helps children develop their writing and argumentation skills through interactive storytelling games and debates. Built with Next.js 15, TypeScript, and Prisma.

## 🎮 Features

### For Children
- **Parcours à Histoire**: Step-by-step story creation with AI guidance
- **La Roue à Histoires**: Random story element generator for creative writing
- **L'Argumentarium**: Structured debate practice with AI feedback
- **Progress Tracking**: XP system, levels, and achievement badges
- **Creative Dashboard**: Personalized interface with pastel themes

### For Parents
- **Progress Monitoring**: Track children's writing development
- **Content Overview**: View completed stories and debate performance
- **Subscription Management**: Handle premium features and billing
- **Family Dashboard**: Manage multiple children's accounts

### For Administrators
- **Analytics Dashboard**: Comprehensive KPIs and user metrics
- **Interactive Charts**: Revenue, user growth, and content activity trends
- **Content Moderation**: Review and manage user-generated content
- **User Management**: Oversee platform usage and subscriptions

## 🚀 Quick Start

### 🎯 Demo Setup (Recommended)

Get Scribilis running with one command:

```bash
# Run complete demo setup with PostgreSQL Docker + fake data
./demo.sh

# Or reset and regenerate demo data
./demo.sh --reset-data
```

This will:
- ✅ Start PostgreSQL in Docker
- ✅ Setup environment configuration
- ✅ Install dependencies
- ✅ Create database schema
- ✅ Seed admin user and realistic demo data
- ✅ Launch the application

**Demo Accounts:**
- Admin: `admin@scribilis.com` / `demo123`
- Parent: `parent@demo.com` / `demo123`
- Child: `emma` / `demo123`

See [DEMO-SETUP.md](./DEMO-SETUP.md) for detailed demo instructions.

### 🔧 Manual Installation

If you prefer manual setup:

#### Prerequisites
- Node.js 18+
- PostgreSQL database
- Docker (for easy PostgreSQL setup)
- npm or yarn package manager

#### Installation Steps

```bash
# Clone the repository
git clone https://github.com/your-org/scribilis.git
cd scribilis

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your database URL and secrets

# Set up PostgreSQL (using Docker - optional)
./scripts/db-setup.sh

# Generate Prisma client
npm run db:generate

# Run database migrations
npm run db:migrate

# Seed the database with admin user
npm run seed

# Start development server
npm run dev
```

Visit `http://localhost:3000` to see the application.

## 🧪 Development

### Database Management

```bash
# Reset database (CAUTION: Deletes all data)
npm run db:reset

# Apply new migrations
npm run db:migrate

# Open Prisma Studio (Database GUI)
npm run db:studio

# Seed fake data for development/testing
npm run seed:admin
```

### Testing

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Generate coverage report
npm run test:coverage
```

### Code Quality

```bash
# Run ESLint
npm run lint

# Type checking
npx tsc --noEmit

# Build for production
npm run build
```

## 🌍 Production Deployment

### Pre-Launch Checklist

Before launching to production, ensure all fake data is removed:

```bash
# Check current database state
npm run pre-launch:check

# Remove all fake/demo data (requires confirmation)
npm run cleanup:fake-data -- --confirm

# Verify cleanup completed
npm run pre-launch:check
```

### Environment Setup

Required environment variables for production:

```env
NODE_ENV=production
DATABASE_URL="postgresql://user:password@host:port/database"
JWT_SECRET="your-super-secret-jwt-key"
NEXTAUTH_SECRET="your-nextauth-secret"
```

### Deployment Commands

```bash
# Build for production
npm run build

# Start production server
npm start

# Or deploy to your hosting platform
# (Vercel, Railway, AWS, etc.)
```

## 🎨 Architecture

### Tech Stack
- **Frontend**: Next.js 15, React 19, TypeScript
- **Styling**: Tailwind CSS with custom pastel theme
- **Backend**: Next.js API Routes
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: JWT with custom middleware
- **UI Components**: shadcn/ui with custom themes
- **Charts**: Recharts for admin analytics
- **Testing**: Jest with React Testing Library

### Project Structure

```
src/
├── app/                    # Next.js 15 app router
│   ├── (auth)/            # Authentication pages
│   ├── admin/             # Admin dashboard
│   ├── kids/              # Children's interface
│   ├── parents/           # Parent dashboard
│   └── api/               # API endpoints
├── components/            # Reusable React components
│   ├── ui/                # Base UI components (shadcn)
│   ├── admin/             # Admin-specific components
│   ├── games/             # Game-specific components
│   └── layout/            # Layout components
├── lib/                   # Utility libraries
│   ├── auth/              # Authentication logic
│   ├── database/          # Database utilities
│   └── utils/             # Helper functions
└── styles/                # Global styles and themes
```

## 🎮 Game Mechanics

### Parcours à Histoire
- Step-by-step story creation
- AI-guided prompts and suggestions
- Character and setting development
- Plot progression with choices
- Final story compilation

### La Roue à Histoires
- Random element generator
- Character, setting, object combinations
- Creative writing prompts
- Timed writing challenges
- Story variation mechanics

### L'Argumentarium
- Structured debate framework
- Topic selection system
- Argument construction help
- Counter-argument practice
- AI feedback and scoring

### Progression System
- XP points for completed activities
- Level progression (1-10+)
- Achievement badges
- Creativity metrics
- Writing skill development tracking

## 👥 User Roles

### Children (`CHILD`)
- Access to all games and activities
- Progress tracking and achievements
- Content creation and saving
- Limited administrative controls

### Parents (`PARENT`)
- Child account management
- Progress monitoring
- Subscription handling
- Content oversight

### Administrators (`ADMIN`)
- Full platform access
- User management
- Content moderation
- Analytics and reporting
- System configuration

## 🔒 Security

### Authentication
- JWT-based authentication
- Role-based access control
- Secure password hashing (bcryptjs)
- Session management

### Data Protection
- Input validation and sanitization
- SQL injection prevention (Prisma)
- XSS protection
- CORS configuration
- Rate limiting ready

### Privacy
- Child-safe content policies
- GDPR compliance preparation
- Data minimization
- Parental consent mechanisms

## 📊 Analytics & Monitoring

### Admin Dashboard KPIs
- Total users, parents, and children
- Content creation metrics
- Subscription and revenue data
- User engagement analytics
- Growth rate calculations

### Performance Monitoring
- API response times
- Database query optimization
- Frontend performance metrics
- Error tracking and logging

## 🚧 Development Workflow

### Data Management

```bash
# Development with fake data
npm run seed:admin           # Add realistic fake data
npm run dev                  # Develop with populated database

# Testing
npm test                     # Run test suite
npm run test:coverage        # Check test coverage

# Pre-production cleanup
npm run cleanup:fake-data -- --confirm    # Remove all fake data
npm run pre-launch:check                  # Verify ready for launch
```

### Git Workflow
1. Create feature branch from `main`
2. Develop and test locally
3. Submit pull request
4. Code review and approval
5. Merge to `main`
6. Deploy to staging
7. UAT and final testing
8. Deploy to production

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Use TypeScript for all new code
- Follow existing code style and conventions
- Write tests for new features
- Update documentation as needed
- Ensure accessibility standards (WCAG 2.1)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

### Development Team
- Technical Lead: [Contact information]
- Frontend Developer: [Contact information]
- Backend Developer: [Contact information]
- UI/UX Designer: [Contact information]

### Resources
- [Issue Tracker](https://github.com/your-org/scribilis/issues)
- [Discussions](https://github.com/your-org/scribilis/discussions)
- [Documentation](./docs/)
- [Changelog](./CHANGELOG.md)

## 🎯 Roadmap

### Phase 1 (Current)
- ✅ Core storytelling games
- ✅ User authentication and roles
- ✅ Admin dashboard with analytics
- ✅ Subscription management
- ✅ Progress tracking system

### Phase 2 (Q2 2026)
- 🔄 Mobile app development
- 🔄 Advanced AI integration
- 🔄 Multiplayer storytelling
- 🔄 Teacher dashboard
- 🔄 Curriculum integration

### Phase 3 (Q3 2026)
- 📋 Social features (friend connections)
- 📋 Story sharing and publication
- 📋 Advanced analytics
- 📋 Multi-language support
- 📋 API for third-party integrations

---

**Made with ❤️ for young storytellers** 🌟
