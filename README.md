[![IconikStudio](https://img.shields.io/badge/IconikStudio-Follow%20Us-E4405F?logo=instagram&logoColor=white)](https://www.instagram.com/iconikstudio.in/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![PWA](https://img.shields.io/badge/PWA-5A0FC8?logo=pwa&logoColor=white)](https://web.dev/progressive-web-apps/)
[![Phaser](https://img.shields.io/badge/Phaser-8A2BE2?logo=phaser&logoColor=white)](https://phaser.io/)
[![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen)](https://github.com/mjeshani2/iconikLearn)

---

# Gamified Environmental Education Platform

An enterprise-grade digital platform transforming environmental education in Indian schools and colleges through gamification, real-world challenges, and measurable behavioral change.

## Overview

The Gamified Environmental Education Platform addresses the critical gap in environmental education by moving beyond theoretical learning to create an interactive, engaging, and experiential learning ecosystem. Built for scale, the platform supports millions of students across India while maintaining offline-first capabilities for low-connectivity regions.

Aligned with India's National Education Policy (NEP) 2020, Sustainable Development Goals (SDGs), and national environmental initiatives, this platform empowers students to become active environmental stewards through points, badges, leaderboards, and tangible rewards.

## Problem Statement

Environmental education in Indian schools and colleges is predominantly textbook-based with minimal practical application. Students lack motivation to adopt sustainable habits, and institutions have no systematic way to track or measure environmental actions. This platform bridges that gap by gamifying environmental learning and validating real-world eco-actions.

## Key Features

### For Students

- **Gamified Learning**: Earn points, badges, and levels by completing environmental challenges
- **Real-World Challenges**: Daily, weekly, and special event eco-tasks across 6 categories (Energy, Water, Waste, Biodiversity, Transportation, Food)
- **Leaderboards**: Compete with classmates, school, district, state, and national rankings
- **Streaks & Rewards**: Maintain daily streaks for bonus points and redeem eco-coins for real rewards
- **Interactive Content**: Bite-sized lessons, quizzes, videos, and infographics in multiple languages
- **Social Features**: Join eco-clubs, challenge friends, share achievements, and collaborate on group tasks
- **Offline Mode**: Download challenges and sync submissions when connectivity is restored
- **Impact Tracking**: Personal carbon footprint calculator and environmental impact dashboard

### For Teachers

- **Class Management**: Manage multiple classes, assign challenges, and track student participation
- **Verification Dashboard**: Review and approve student submissions with AI-assisted suggestions
- **Custom Challenges**: Create institution-specific eco-tasks aligned with curriculum
- **Analytics**: Monitor class engagement, identify at-risk students, and generate progress reports
- **Curriculum Integration**: Map challenges to NEP 2020 environmental education standards
- **Communication Tools**: Send announcements, share resources, and engage with parents

### For Administrators

- **Institution Dashboard**: Real-time metrics on participation, environmental impact, and ROI
- **User Management**: Role-based access control, bulk user import, and account administration
- **Leaderboard Configuration**: Set up inter-school competitions and award recognitions
- **Content Management**: Curate challenge libraries and approve teacher-created content
- **Compliance Reporting**: Generate reports for regulatory requirements and stakeholder communication
- **Integration Hub**: Connect with student information systems, LMS, and external APIs

### For NGOs & Government

- **Campaign Management**: Create regional or national environmental campaigns
- **Impact Measurement**: Aggregate environmental impact data across institutions and regions
- **Expert Validation**: Provide verification for high-value challenges and contribute content
- **Resource Allocation**: Distribute grants, funding, and physical resources to institutions
- **Policy Insights**: Access data for research, policy-making, and program evaluation


## Platform Screenshots

> Screenshots will be added as the platform is developed

### Student Dashboard
*Coming soon: Personalized dashboard with points, level, badges, and recommended challenges*

### Challenge Browser
*Coming soon: Browse and filter environmental challenges by category and difficulty*

### Submission Flow
*Coming soon: Upload evidence and submit challenge completion*

### Leaderboards
*Coming soon: Real-time rankings across class, school, and national levels*

### Teacher Verification Portal
*Coming soon: Review and approve student submissions with AI assistance*

### Institution Analytics
*Coming soon: Comprehensive dashboard with engagement and impact metrics*

---

## Tech Stack

### Frontend - Progressive Web App (PWA)
- **Core**: HTML5, CSS3, Vanilla JavaScript (ES6+)
- **Architecture**: Offline-first PWA with Service Workers
- **Game Engine**: Phaser.js (2D educational games)
- **Animation**: CreateJS for interactive content
- **Graphics**: HTML5 Canvas/SVG for visualizations
- **Charts**: Chart.js for analytics and data visualization
- **Maps**: Leaflet with OpenStreetMap (free)
- **Offline Storage**: IndexedDB + LocalStorage + Cache API
- **Internationalization**: i18next (vanilla JS compatible)
- **Web App Manifest**: Installable app experience

**Why Vanilla JavaScript PWA?**
- Smaller bundle size (<500KB vs 2MB+ frameworks)
- Faster performance on low-end devices
- Works as web and installable mobile app
- Complete offline functionality
- No app store dependency
- Efficient on low-cost Android phones and laptops

### Backend (Lightweight & Scalable)
- **Runtime**: Node.js 20 LTS
- **Framework**: Express.js (minimal, fast)
- **Language**: JavaScript (ES6+)
- **Authentication**: JWT (jsonwebtoken)
- **Validation**: Joi
- **Database Access**: pg (PostgreSQL native driver)
- **File Upload**: Multer
- **Task Queue**: Simple queue with Redis (optional)
- **Cron Jobs**: node-cron
- **Testing**: Jest

**Why Lightweight Backend?**
- Most learning activities work offline
- Backend primarily for progress sync, teacher analytics, content updates
- Minimal server load due to offline-first architecture

### Databases
- **Primary**: PostgreSQL 15 (recommended) or MySQL 8.0
- **Offline Sync**: CouchDB + PouchDB (optional, excellent offline-first sync)
- **Cache**: Redis 7.2 (optional, for high-traffic scenarios)
- **Client-Side**: IndexedDB, LocalStorage, Cache API

### Gamification & Learning
- **Game Engine**: Phaser.js (2D educational games)
- **Animations**: CreateJS
- **XP System**: Custom JavaScript module
- **Leaderboards**: Local classroom competition with online sync
- **Achievement Engine**: Custom JavaScript for educational milestones

### Low-Cost/Free Deployment
**Frontend (PWA)**:
- Netlify (free tier with global CDN)
- Vercel (optimized hosting with edge functions)
- GitHub Pages (free static hosting)
- Cloudflare Pages (free with CDN)

**Backend Services**:
- Render (simple hosting with PostgreSQL support)
- Railway (developer-friendly with database integration)
- Fly.io (global deployment with edge computing)

**Content Delivery**:
- Cloudflare (free tier CDN for global asset delivery)

### DevOps
- **CI/CD**: GitHub Actions (free for public repos)
- **Containerization**: Docker (optional for backend)
- **Monitoring**: Sentry (free tier), Google Analytics, UptimeRobot

### Third-Party Services
- **Email**: Free SMTP or SendGrid free tier
- **SMS**: Optional via MSG91 (India) or Twilio
- **Push Notifications**: Web Push API (built into PWA)
- **Maps**: Leaflet + OpenStreetMap (completely free)
- **AI/ML**: TensorFlow.js (client-side, Phase 2)
- **Speech**: Web Speech API (browser-native)

---

## Folder Structure

```
eco-platform/
├── frontend/                   # Progressive Web App (PWA)
│   ├── index.html             # Main HTML entry point
│   ├── manifest.json          # Web App Manifest
│   ├── sw.js                  # Service Worker
│   ├── css/
│   │   ├── main.css           # Main styles
│   │   ├── responsive.css     # Mobile-first responsive design
│   │   └── animations.css     # Lightweight animations
│   ├── js/
│   │   ├── app.js             # Main application logic
│   │   ├── modules/
│   │   │   ├── auth.js        # Authentication module
│   │   │   ├── challenges.js  # Challenge management
│   │   │   ├── gamification.js # XP, badges, leaderboards
│   │   │   ├── offline.js     # Offline sync logic
│   │   │   └── i18n.js        # Internationalization
│   │   ├── games/
│   │   │   ├── phaser-config.js # Phaser.js setup
│   │   │   └── educational-games/ # Game modules
│   │   ├── utils/
│   │   │   ├── storage.js     # IndexedDB/LocalStorage helpers
│   │   │   ├── api.js         # API client
│   │   │   └── helpers.js     # Utility functions
│   │   └── lib/
│   │       ├── phaser.min.js  # Phaser.js library
│   │       ├── chart.min.js   # Chart.js library
│   │       ├── leaflet.min.js # Leaflet maps
│   │       └── i18next.min.js # i18next library
│   ├── assets/
│   │   ├── images/            # Optimized images
│   │   ├── icons/             # App icons
│   │   ├── games/             # Game assets
│   │   └── locales/           # Language JSON files
│   ├── pages/
│   │   ├── student/           # Student interface pages
│   │   ├── teacher/           # Teacher portal pages
│   │   └── admin/             # Admin dashboard pages
│   └── tests/
│       └── unit/              # JavaScript unit tests
│
├── backend/                   # Node.js API Service
│   ├── src/
│   │   ├── routes/
│   │   │   ├── auth.js        # Authentication routes
│   │   │   ├── users.js       # User management
│   │   │   ├── challenges.js  # Challenge CRUD
│   │   │   ├── submissions.js # Submission handling
│   │   │   ├── gamification.js # Points, badges, leaderboards
│   │   │   └── analytics.js   # Analytics endpoints
│   │   ├── middleware/
│   │   │   ├── auth.js        # JWT verification
│   │   │   ├── validation.js  # Input validation
│   │   │   └── errorHandler.js # Error handling
│   │   ├── models/
│   │   │   ├── user.js        # User model
│   │   │   ├── challenge.js   # Challenge model
│   │   │   └── submission.js  # Submission model
│   │   ├── services/
│   │   │   ├── authService.js
│   │   │   ├── gamificationService.js
│   │   │   └── syncService.js
│   │   ├── utils/
│   │   │   ├── db.js          # Database connection
│   │   │   ├── jwt.js         # JWT utilities
│   │   │   └── helpers.js     # Helper functions
│   │   ├── config/
│   │   │   └── index.js       # Configuration
│   │   └── app.js             # Express app setup
│   ├── tests/
│   │   └── api/               # API tests
│   ├── package.json
│   └── .env.example
│
├── database/
│   ├── migrations/            # Database migrations
│   ├── seeds/                 # Seed data
│   └── schema.sql             # PostgreSQL schema
│
├── docs/
│   ├── requirements.md        # Product requirements
│   ├── design.md              # System architecture
│   ├── api.md                 # API documentation
│   └── deployment.md          # Deployment guide
│
├── .github/
│   └── workflows/
│       ├── frontend-deploy.yml # Frontend CI/CD
│       └── backend-deploy.yml  # Backend CI/CD
│
├── docker-compose.yml         # Local development (optional)
├── package.json               # Root package.json
└── README.md
```

---

## Installation & Setup

### Prerequisites

- Node.js 20 LTS or higher
- PostgreSQL 15 (or MySQL 8.0)
- Git
- A modern web browser (Chrome, Firefox, Edge, Safari)
- Optional: Docker for containerized development

### Local Development Setup

1. **Clone the repository**
```bash
git clone https://github.com/mjeshani2/iconikLearn.git
cd iconikLearn
```

2. **Set up the database**
```bash
# Install PostgreSQL locally or use Docker
docker run --name eco-postgres -e POSTGRES_PASSWORD=password -p 5432:5432 -d postgres:15

# Create database
psql -U postgres -c "CREATE DATABASE eco_platform;"

# Run migrations
cd backend
npm install
npm run migrate
```

3. **Set up backend**
```bash
cd backend

# Install dependencies
npm install

# Copy environment file
cp .env.example .env

# Edit .env with your configuration
# DATABASE_URL=postgresql://postgres:password@localhost:5432/eco_platform
# JWT_SECRET=your-secret-key-here
# PORT=4000

# Seed initial data (optional)
npm run seed

# Start backend server
npm run dev
# Backend running on http://localhost:4000
```

4. **Set up frontend**
```bash
cd frontend

# No build step needed for vanilla JavaScript!
# Just serve the files with a local server

# Option 1: Using Python
python -m http.server 3000

# Option 2: Using Node.js http-server
npx http-server -p 3000

# Option 3: Using VS Code Live Server extension
# Right-click index.html and select "Open with Live Server"
```

5. **Access the application**
- Frontend: http://localhost:3000
- Backend API: http://localhost:4000/api/v1
- API Health: http://localhost:4000/health

6. **Test offline functionality**
- Open the app in browser
- Open DevTools > Application > Service Workers
- Check "Offline" to simulate offline mode
- App should continue working with cached data


---

## Environment Variables

### Frontend Configuration (frontend/js/config.js)

```javascript
// API Configuration
const CONFIG = {
  API_URL: 'http://localhost:4000/api/v1',
  API_TIMEOUT: 10000,
  
  // Offline Configuration
  ENABLE_OFFLINE: true,
  CACHE_VERSION: 'v1',
  CACHE_EXPIRY: 7 * 24 * 60 * 60 * 1000, // 7 days
  
  // Gamification
  POINTS_PER_CHALLENGE: 100,
  STREAK_BONUS_MULTIPLIER: 1.1,
  
  // Storage
  DB_NAME: 'eco_platform',
  DB_VERSION: 1,
  
  // Maps (OpenStreetMap - no API key needed)
  MAP_TILE_URL: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
  
  // Analytics (optional)
  GA_ID: 'G-XXXXXXXXXX', // Google Analytics ID
  
  // Feature Flags
  ENABLE_GAMES: true,
  ENABLE_VOICE: true,
  ENABLE_LOCATION: true
};
```

### Backend Configuration (backend/.env)

```bash
# Server
NODE_ENV=development
PORT=4000
API_VERSION=v1

# Database
DATABASE_URL=postgresql://postgres:password@localhost:5432/eco_platform
# Alternative: MySQL
# DATABASE_URL=mysql://root:password@localhost:3306/eco_platform

# Authentication
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
JWT_EXPIRES_IN=15m
REFRESH_TOKEN_EXPIRES_IN=7d

# CORS
CORS_ORIGIN=http://localhost:3000

# File Upload
UPLOAD_DIR=./uploads
MAX_FILE_SIZE=10485760  # 10MB

# Email (Optional - use free SMTP or SendGrid free tier)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password
FROM_EMAIL=noreply@ecoplatform.in

# SMS (Optional - for OTP)
SMS_PROVIDER=msg91  # or twilio
SMS_API_KEY=your-sms-api-key
SMS_SENDER_ID=ECOPLT

# Redis (Optional - for high-traffic scenarios)
# REDIS_URL=redis://localhost:6379

# Rate Limiting
RATE_LIMIT_WINDOW=15  # minutes
RATE_LIMIT_MAX_REQUESTS=100

# Logging
LOG_LEVEL=info
LOG_FILE=./logs/app.log

# Security
BCRYPT_ROUNDS=10
SESSION_SECRET=your-session-secret-change-this
```

### Database Configuration (database/config.js)

```javascript
module.exports = {
  development: {
    client: 'postgresql',
    connection: {
      host: 'localhost',
      port: 5432,
      database: 'eco_platform',
      user: 'postgres',
      password: 'password'
    },
    migrations: {
      directory: './migrations'
    },
    seeds: {
      directory: './seeds'
    }
  },
  production: {
    client: 'postgresql',
    connection: process.env.DATABASE_URL,
    ssl: { rejectUnauthorized: false }
  }
};
```

---

## Local Development Guide

### Running Tests

```bash
# Backend tests
cd backend
npm test

# Run tests in watch mode
npm run test:watch

# Generate coverage report
npm run test:coverage

# Frontend tests (using Jest)
cd frontend
npm test
```

### Database Management

```bash
cd backend

# Create a new migration
npm run migrate:create add_user_preferences

# Run migrations
npm run migrate

# Rollback last migration
npm run migrate:rollback

# Reset database (caution: deletes all data)
npm run db:reset

# Seed database with sample data
npm run seed
```

### Code Quality

```bash
# Lint JavaScript code
npm run lint

# Fix linting issues
npm run lint:fix

# Format code
npm run format
```

### Building for Production

```bash
# Backend: No build step needed (vanilla Node.js)
cd backend
npm install --production

# Frontend: Minify JavaScript and CSS
cd frontend
npm run build  # Minifies JS/CSS, optimizes images

# The build script will:
# - Minify all JavaScript files
# - Minify and combine CSS files
# - Optimize images (WebP conversion)
# - Generate service worker with cache manifest
# - Output to /dist folder
```

### PWA Installation Testing

```bash
# Test PWA installation locally
# 1. Serve frontend over HTTPS (required for PWA)
npx http-server frontend -p 3000 --ssl

# 2. Open in Chrome: https://localhost:3000
# 3. Check DevTools > Application > Manifest
# 4. Click "Install" button in address bar
# 5. Test offline mode in DevTools > Network > Offline
```

---

## Deployment Instructions

### Production Deployment

**Frontend Deployment (Netlify/Vercel)**:

1. **Connect repository to Netlify**
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login to Netlify
netlify login

# Initialize site
cd frontend
netlify init

# Deploy
netlify deploy --prod
```

2. **Or use Vercel**
```bash
# Install Vercel CLI
npm install -g vercel

# Login and deploy
cd frontend
vercel --prod
```

3. **Configure custom domain** (optional)
- Add custom domain in Netlify/Vercel dashboard
- Update DNS records
- SSL certificate automatically provisioned

**Backend Deployment (Render/Railway)**:

1. **Deploy to Render**
```bash
# Create render.yaml in backend folder
# Push to GitHub
# Connect repository in Render dashboard
# Render will auto-deploy on push
```

2. **Or deploy to Railway**
```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Initialize and deploy
cd backend
railway init
railway up
```

3. **Set environment variables**
- Add all .env variables in platform dashboard
- Configure DATABASE_URL from managed PostgreSQL
- Set JWT_SECRET and other secrets

4. **Run database migrations**
```bash
# SSH into backend service
railway run npm run migrate

# Or use Render shell
# Navigate to Shell tab and run: npm run migrate
```

**Database Setup**:

1. **Use managed PostgreSQL**
- Render PostgreSQL (free tier available)
- Railway PostgreSQL (included)
- Supabase (free tier with extras)
- ElephantSQL (free tier)

2. **Configure connection**
- Copy DATABASE_URL from provider
- Add to backend environment variables
- Test connection

**CDN Setup (Cloudflare)**:

1. **Add site to Cloudflare**
- Sign up for free account
- Add your domain
- Update nameservers

2. **Configure caching**
- Enable "Always Online" for offline fallback
- Set cache rules for static assets
- Enable Brotli compression

**Monitoring**:

1. **Set up Sentry** (free tier)
```bash
# Add to frontend
<script src="https://browser.sentry-cdn.com/7.x.x/bundle.min.js"></script>
<script>
  Sentry.init({ dsn: 'your-dsn-here' });
</script>
```

2. **Set up UptimeRobot** (free)
- Add monitors for frontend and backend
- Configure email alerts
- Set check interval to 5 minutes

**Verify Deployment**:
```bash
# Check frontend
curl https://your-domain.com

# Check backend health
curl https://api.your-domain.com/health

# Test PWA installation
# Open site in Chrome, click Install button
```

1. **Set up AWS infrastructure**
```bash
cd infrastructure/terraform
terraform init
terraform plan
terraform apply
```

2. **Configure secrets**
```bash
# Store secrets in AWS Secrets Manager
aws secretsmanager create-secret --name eco-platform/prod/api --secret-string file://secrets.json
```

3. **Build and push Docker images**
```bash
# Build images
docker-compose -f docker-compose.prod.yml build

# Tag images
docker tag eco-platform-web:latest <ecr-repo>/eco-platform-web:latest
docker tag eco-platform-api:latest <ecr-repo>/eco-platform-api:latest

# Push to ECR
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin <ecr-repo>
docker push <ecr-repo>/eco-platform-web:latest
docker push <ecr-repo>/eco-platform-api:latest
```

4. **Deploy to ECS**
```bash
# Update ECS services
aws ecs update-service --cluster eco-platform-prod --service web --force-new-deployment
aws ecs update-service --cluster eco-platform-prod --service api --force-new-deployment
```

5. **Run database migrations**
```bash
# Connect to ECS task and run migrations
aws ecs execute-command --cluster eco-platform-prod --task <task-id> --command "npm run migrate"
```

6. **Verify deployment**
```bash
# Check service health
curl https://api.ecoplatform.in/health

# Monitor logs
aws logs tail /ecs/eco-platform-api --follow
```

### Staging Deployment

Staging deployments are automated via GitHub Actions on push to `staging` branch.

### Rollback Procedure

```bash
# Rollback to previous task definition
aws ecs update-service --cluster eco-platform-prod --service api --task-definition eco-platform-api:previous

# Or use blue-green deployment rollback
# Switch traffic back to blue environment via load balancer
```

---

## User Roles Overview

### Student
- Register and create profile
- Browse and accept challenges
- Submit challenge completions with evidence
- Earn points, badges, and levels
- View leaderboards and rankings
- Access learning content and quizzes
- Join eco-clubs and teams
- Redeem rewards

### Teacher / Eco-Coordinator
- All student permissions
- Manage classes and students
- Create custom challenges
- Verify student submissions
- View class analytics and reports
- Communicate with students and parents
- Assign mandatory challenges

### School/College Administrator
- All teacher permissions
- Manage institution profile and settings
- Add and manage teacher accounts
- View institution-wide analytics
- Configure leaderboards and competitions
- Manage content library
- Generate compliance reports
- Access API integrations

### NGO Partner
- Create regional/national campaigns
- Contribute expert content
- Provide expert validation for challenges
- View aggregated impact data
- Manage resource allocation
- Access research data

### Government Official
- View district/state/national analytics
- Generate policy reports
- Monitor program implementation
- Access aggregated environmental impact data
- Manage government campaigns

---

## Contribution Guidelines

We welcome contributions from the community! Please follow these guidelines:

### Getting Started

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Write or update tests
5. Ensure all tests pass (`npm run test`)
6. Commit your changes (`git commit -m 'Add amazing feature'`)
7. Push to the branch (`git push origin feature/amazing-feature`)
8. Open a Pull Request

### Code Standards

- Follow the existing code style
- Write meaningful commit messages
- Add comments for complex logic
- Update documentation as needed
- Ensure TypeScript types are properly defined
- Write unit tests for new features
- Keep functions small and focused

### Pull Request Process

1. Update README.md with details of changes if applicable
2. Update the documentation in `/docs` if needed
3. Ensure CI/CD pipeline passes
4. Request review from maintainers
5. Address review feedback
6. Squash commits before merging

### Reporting Bugs

Use the GitHub issue tracker to report bugs. Include:
- Clear description of the issue
- Steps to reproduce
- Expected vs actual behavior
- Screenshots if applicable
- Environment details (OS, browser, versions)

### Suggesting Features

Open an issue with the "feature request" label. Include:
- Clear description of the feature
- Use cases and benefits
- Potential implementation approach
- Mockups or examples if applicable

---

## Roadmap

### Phase 1: MVP (Months 1-6)
- [x] Requirements and design documentation
- [ ] Core platform development
  - [ ] User authentication and profiles (JWT-based)
  - [ ] Challenge system with offline support
  - [ ] Gamification engine (XP, badges, leaderboards)
  - [ ] Basic verification workflow
  - [ ] Educational mini-games (Phaser.js)
- [ ] PWA with offline capabilities (Service Workers, IndexedDB)
- [ ] Teacher and admin portals (vanilla JavaScript)
- [ ] Basic analytics dashboard (Chart.js)
- [ ] Pilot deployment (100 schools on free hosting)

### Phase 2: Scale & Enhance (Months 7-12)
- [ ] Client-side AI verification (TensorFlow.js)
- [ ] Advanced analytics and insights
- [ ] Regional language support (10+ languages with downloadable packs)
- [ ] Enhanced social features and eco-clubs
- [ ] CouchDB/PouchDB for advanced offline sync
- [ ] Web Speech API for voice-based learning
- [ ] Integration with LMS platforms (simple REST API)
- [ ] Scale to 10,000 schools

### Phase 3: Advanced Features (Months 13-18)
- [ ] Advanced Phaser.js educational games
- [ ] CreateJS animations for interactive content
- [ ] Peer-to-peer local content sharing
- [ ] SMS-based sync for extreme low connectivity
- [ ] API marketplace for third-party developers
- [ ] Corporate partnership program
- [ ] Scale to 50,000 institutions

### Future Considerations
- Native mobile apps (optional, if PWA limitations found)
- IoT device integration (smart meters, sensors)
- Blockchain-based certificates
- Global environmental challenges
- Research collaboration platform

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- National Education Policy (NEP) 2020 for environmental education guidelines
- United Nations Sustainable Development Goals (SDGs)
- Ministry of Environment, Forest and Climate Change, Government of India
- Educational institutions participating in pilot programs
- Open-source community for amazing tools and libraries

---

## Contact & Support

- **Website**: https://ecoplatform.in
- **Email**: support@ecoplatform.in
- **Documentation**: https://docs.ecoplatform.in
- **Community Forum**: https://community.ecoplatform.in
- **Twitter**: @EcoPlatformIN
- **LinkedIn**: IconikStudio

For enterprise inquiries: enterprise@ecoplatform.in

---

<div align="center">

### Built with Excellence by IconikStudio

**Empowering global education through innovative, accessible, offline-first learning technology**

[![GitHub Stars](https://img.shields.io/github/stars/mjeshani2/iconikLearn?style=social)](https://github.com/mjeshani2/iconikLearn/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/mjeshani2/iconikLearn?style=social)](https://github.com/mjeshani2/iconikLearn/network/members)
[![GitHub Issues](https://img.shields.io/github/issues/mjeshani2/iconikLearn)](https://github.com/mjeshani2/iconikLearn/issues)
[![GitHub Pull Requests](https://img.shields.io/github/issues-pr/mjeshani2/iconikLearn)](https://github.com/mjeshani2/iconikLearn/pulls)

**[⭐ Star us on GitHub](https://github.com/mjeshani2/iconikLearn)** • **[🐛 Report Bug](https://github.com/mjeshani2/iconikLearn/issues/new?template=bug_report.md)** • **[✨ Request Feature](https://github.com/mjeshani2/iconikLearn/issues/new?template=feature_request.md)**

*Making quality education accessible, engaging, and effective for learners worldwide.*

</div>
