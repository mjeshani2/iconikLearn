# Enterprise System & Architecture Design
## Gamified Environmental Education Platform

---

## High-Level System Architecture

### Architecture Deployment Modes

This platform supports two officially supported deployment modes:

#### Mode A: Lightweight Institutional Deployment (Default)
- Single Node.js backend
- PostgreSQL
- Vanilla JS PWA (offline-first)
- Designed for schools, colleges, NGOs, rural connectivity
- Minimal infrastructure cost
- Most learning & gameplay runs offline

#### Mode B: Enterprise / Government Deployment
- Microservices architecture
- Kafka + Redis + Kubernetes
- Horizontal scaling across regions
- Used for state / national-level rollouts
- Advanced analytics & reporting

Both modes share the same core domain logic and APIs.

### Architecture Overview

The platform follows a cloud-native, microservices-based architecture with offline-first capabilities. The system is designed for horizontal scalability, high availability, and resilience.

**Architecture Layers**:

1. **Presentation Layer**: Progressive Web App (PWA), Native Mobile Apps (optional)
2. **API Gateway Layer**: Request routing, authentication, rate limiting, caching
3. **Application Layer**: Microservices for core business logic
4. **Data Layer**: Distributed databases, caching, message queues
5. **Integration Layer**: Third-party services, external APIs, webhooks
6. **Analytics Layer**: Data warehouse, real-time analytics, reporting
7. **Infrastructure Layer**: Cloud services, CDN, monitoring, security

### System Components

**Client Applications**:
- Progressive Web App (React/Next.js)
- Android App (React Native - optional phase 2)
- iOS App (React Native - optional phase 2)
- Admin Dashboard (React/Next.js)
- Teacher Portal (React/Next.js)

**Backend Services**:
- User Service (authentication, profiles, roles)
- Challenge Service (tasks, submissions, verification)
- Gamification Service (points, badges, levels, leaderboards)
- Content Service (learning materials, resources)
- Analytics Service (metrics, reports, insights)
- Notification Service (push, email, SMS)
- Media Service (image/video upload, processing, storage)
- Integration Service (external APIs, webhooks)

**Data Stores**:
- PostgreSQL (relational data: users, institutions, challenges)
- MongoDB (document data: submissions, content, logs)
- Redis (caching, sessions, real-time leaderboards)
- Elasticsearch (search, analytics, logging)
- S3-compatible storage (media files, backups)

**Infrastructure Services**:
- API Gateway (Kong/AWS API Gateway)
- Message Queue (RabbitMQ/AWS SQS)
- CDN (CloudFront/Cloudflare)
- Load Balancer (AWS ALB/Nginx)
- Container Orchestration (Kubernetes/ECS)


### Network Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Client Layer (PWA/Apps)                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ Student  │  │ Teacher  │  │  Admin   │  │   NGO    │   │
│  │   App    │  │  Portal  │  │Dashboard │  │  Portal  │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↓ HTTPS
┌─────────────────────────────────────────────────────────────┐
│                    CDN + Edge Caching                        │
│              (CloudFront / Cloudflare)                       │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                   Load Balancer (ALB)                        │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                    API Gateway (Kong)                        │
│  Authentication │ Rate Limiting │ Caching │ Routing         │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                  Microservices Layer                         │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐   │
│  │User  │ │Chall │ │Gamif │ │Cont  │ │Analy │ │Notif │   │
│  │Svc   │ │enge  │ │ication│ │ent   │ │tics  │ │ication│  │
│  └──────┘ └──────┘ └──────┘ └──────┘ └──────┘ └──────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                      Data Layer                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │PostgreSQL│  │ MongoDB  │  │  Redis   │  │Elastic   │   │
│  │(Primary) │  │(Documents│  │ (Cache)  │  │search    │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## Component-Level Design

### User Service

**Responsibilities**:
- User registration and authentication
- Profile management
- Role-based access control
- Session management
- Password reset and security

**Key APIs**:
- `POST /api/v1/auth/register` - User registration
- `POST /api/v1/auth/login` - User login
- `POST /api/v1/auth/refresh` - Token refresh
- `GET /api/v1/users/{id}` - Get user profile
- `PUT /api/v1/users/{id}` - Update profile
- `GET /api/v1/users/{id}/achievements` - Get user achievements

**Database Schema**:
- users (id, email, phone, password_hash, role, institution_id, created_at)
- profiles (user_id, name, grade, section, avatar, preferences)
- sessions (id, user_id, token, expires_at)
- roles (id, name, permissions)

**Technology**:
- Node.js with Express/Fastify
- JWT for authentication
- bcrypt for password hashing
- PostgreSQL for user data


### Challenge Service

**Responsibilities**:
- Challenge creation and management
- Task assignment and tracking
- Submission handling
- Verification workflow
- Challenge categories and difficulty levels

**Key APIs**:
- `GET /api/v1/challenges` - List challenges (with filters)
- `GET /api/v1/challenges/{id}` - Get challenge details
- `POST /api/v1/challenges` - Create challenge (teacher/admin)
- `POST /api/v1/challenges/{id}/accept` - Accept challenge
- `POST /api/v1/challenges/{id}/submit` - Submit completion proof
- `GET /api/v1/submissions/{id}` - Get submission details
- `PUT /api/v1/submissions/{id}/verify` - Verify submission

**Database Schema**:
- challenges (id, title, description, category, difficulty, points, created_by)
- challenge_assignments (id, challenge_id, user_id, status, accepted_at)
- submissions (id, assignment_id, evidence_type, evidence_url, submitted_at)
- verifications (id, submission_id, verifier_id, status, feedback, verified_at)

**Technology**:
- Node.js with Express
- MongoDB for flexible challenge data
- S3 for evidence storage
- Message queue for async verification

### Gamification Service

**Responsibilities**:
- Points calculation and management
- Badge and achievement system
- Level progression
- Leaderboard generation and updates
- Streak tracking
- Reward management

**Key APIs**:
- `GET /api/v1/gamification/dashboard` - User gamification dashboard
- `GET /api/v1/gamification/leaderboard` - Get leaderboard (with filters)
- `POST /api/v1/gamification/points` - Award points
- `GET /api/v1/gamification/badges` - Get user badges
- `GET /api/v1/gamification/level` - Get user level info
- `POST /api/v1/gamification/rewards/redeem` - Redeem reward

**Database Schema**:
- user_points (user_id, total_points, level, level_progress)
- point_transactions (id, user_id, points, reason, challenge_id, created_at)
- badges (id, name, description, icon, criteria)
- user_badges (user_id, badge_id, earned_at)
- leaderboards (id, type, period, rankings_json, updated_at)
- streaks (user_id, current_streak, longest_streak, last_activity_date)
- rewards (id, name, cost, type, inventory)
- redemptions (id, user_id, reward_id, status, redeemed_at)

**Technology**:
- Node.js with Express
- Redis for real-time leaderboards
- PostgreSQL for transactional data
- Cron jobs for leaderboard updates


### Content Service

**Responsibilities**:
- Learning content management
- Quiz and assessment delivery
- Resource library
- Multi-language content
- Content versioning

**Key APIs**:
- `GET /api/v1/content/lessons` - List lessons
- `GET /api/v1/content/lessons/{id}` - Get lesson details
- `GET /api/v1/content/quizzes/{id}` - Get quiz
- `POST /api/v1/content/quizzes/{id}/submit` - Submit quiz answers
- `GET /api/v1/content/resources` - List resources
- `GET /api/v1/content/search` - Search content

**Database Schema**:
- lessons (id, title, content, category, language, difficulty, created_at)
- quizzes (id, lesson_id, questions_json, passing_score)
- quiz_attempts (id, user_id, quiz_id, score, answers_json, completed_at)
- resources (id, title, type, url, category, language)
- content_progress (user_id, content_id, progress_percent, last_accessed)

**Technology**:
- Node.js with Express
- MongoDB for flexible content structure
- Elasticsearch for content search
- CDN for media delivery

### Analytics Service

**Responsibilities**:
- Data aggregation and processing
- Report generation
- Real-time metrics
- Impact calculations
- Predictive analytics

**Key APIs**:
- `GET /api/v1/analytics/dashboard` - Get dashboard metrics
- `GET /api/v1/analytics/student/{id}` - Student analytics
- `GET /api/v1/analytics/class/{id}` - Class analytics
- `GET /api/v1/analytics/institution/{id}` - Institution analytics
- `GET /api/v1/analytics/impact` - Environmental impact metrics
- `POST /api/v1/analytics/reports/generate` - Generate custom report

**Database Schema**:
- analytics_events (id, event_type, user_id, metadata_json, timestamp)
- daily_metrics (date, user_id, challenges_completed, points_earned, time_spent)
- impact_metrics (id, user_id, co2_saved, water_saved, waste_reduced, calculated_at)
- reports (id, type, parameters_json, file_url, generated_at)

**Technology**:
- Python with FastAPI
- Apache Kafka for event streaming
- Apache Spark for data processing
- PostgreSQL + TimescaleDB for time-series data
- Elasticsearch for analytics queries


### Notification Service

**Responsibilities**:
- Push notifications
- Email notifications
- SMS notifications (optional)
- In-app notifications
- Notification preferences
- Delivery tracking

**Key APIs**:
- `POST /api/v1/notifications/send` - Send notification
- `GET /api/v1/notifications` - Get user notifications
- `PUT /api/v1/notifications/{id}/read` - Mark as read
- `PUT /api/v1/notifications/preferences` - Update preferences
- `POST /api/v1/notifications/subscribe` - Subscribe to push

**Database Schema**:
- notifications (id, user_id, type, title, message, read, created_at)
- notification_preferences (user_id, push_enabled, email_enabled, categories)
- push_subscriptions (user_id, endpoint, keys_json)
- notification_logs (id, notification_id, status, delivered_at)

**Technology**:
- Node.js with Express
- Firebase Cloud Messaging (FCM) for push
- SendGrid/AWS SES for email
- Redis for notification queue
- MongoDB for notification storage

### Media Service

**Responsibilities**:
- File upload handling
- Image/video processing
- Thumbnail generation
- Content moderation
- Storage management

**Key APIs**:
- `POST /api/v1/media/upload` - Upload file
- `GET /api/v1/media/{id}` - Get media metadata
- `DELETE /api/v1/media/{id}` - Delete media
- `POST /api/v1/media/{id}/moderate` - Moderate content

**Database Schema**:
- media_files (id, user_id, type, url, size, moderation_status, uploaded_at)
- thumbnails (media_id, size, url)

**Technology**:
- Node.js with Express
- AWS S3/MinIO for storage
- Sharp for image processing
- FFmpeg for video processing
- AWS Rekognition for content moderation

---

## User Flow Diagrams (Textual)

### Student Challenge Completion Flow

1. Student logs into PWA
2. Views personalized dashboard with recommended challenges
3. Browses challenge categories (Energy, Water, Waste, etc.)
4. Selects a challenge and views details (description, points, difficulty)
5. Accepts challenge (added to "My Challenges")
6. Completes real-world eco-task
7. Opens app and navigates to challenge
8. Clicks "Submit Completion"
9. Uploads evidence (photo/video)
10. Adds description and details
11. Submits for verification
12. Receives confirmation notification
13. Waits for teacher verification (receives notification when verified)
14. Upon approval: Points added, badges unlocked, leaderboard updated
15. Views updated dashboard with new level/achievements


### Teacher Verification Flow

1. Teacher logs into portal
2. Views verification dashboard with pending submissions count
3. Navigates to "Pending Verifications" queue
4. Filters by class, challenge type, or date
5. Selects a submission to review
6. Views student details, challenge requirements, and submitted evidence
7. Reviews photo/video evidence
8. Checks AI verification suggestions (if available)
9. Makes decision:
   - Approve: Adds optional feedback, confirms approval
   - Reject: Provides reason, suggests improvements
   - Request More Info: Asks student for clarification
10. Submits verification decision
11. System updates student points/badges (if approved)
12. Student receives notification
13. Teacher moves to next submission in queue

### Institution Onboarding Flow

1. Admin receives invitation email/link
2. Clicks link and lands on registration page
3. Enters institution details (name, type, location, board)
4. Uploads institution logo and documents
5. Creates admin account (email, password)
6. Verifies email address
7. Completes institution profile setup
8. Configures academic year and terms
9. Adds teacher accounts (bulk upload CSV or individual)
10. Teachers receive invitation emails
11. Teachers complete profile setup
12. Admin creates classes/sections
13. Assigns teachers to classes
14. Uploads student list (bulk CSV import)
15. Students receive invitation codes/links
16. Students register and join classes
17. Admin reviews dashboard and starts first campaign

---

## Data Models

### Core Entities

**User Entity**:
```
User {
  id: UUID (PK)
  email: String (unique)
  phone: String (unique, optional)
  password_hash: String
  role: Enum (student, teacher, admin, ngo, government)
  institution_id: UUID (FK)
  status: Enum (active, inactive, suspended)
  email_verified: Boolean
  created_at: Timestamp
  updated_at: Timestamp
}
```

**Profile Entity**:
```
Profile {
  user_id: UUID (PK, FK)
  first_name: String
  last_name: String
  avatar_url: String
  date_of_birth: Date
  grade: String
  section: String
  gender: Enum (male, female, other, prefer_not_to_say)
  language_preference: String
  bio: Text
  interests: Array<String>
}
```

**Institution Entity**:
```
Institution {
  id: UUID (PK)
  name: String
  type: Enum (school, college, university)
  board: Enum (CBSE, ICSE, State, IB, etc.)
  address: Text
  city: String
  state: String
  pincode: String
  contact_email: String
  contact_phone: String
  logo_url: String
  status: Enum (active, inactive, trial)
  subscription_tier: Enum (free, basic, premium, enterprise)
  created_at: Timestamp
}
```


**Challenge Entity**:
```
Challenge {
  id: UUID (PK)
  title: String
  description: Text
  category: Enum (energy, water, waste, biodiversity, transport, food)
  difficulty: Enum (beginner, intermediate, advanced)
  points: Integer
  estimated_time: Integer (minutes)
  instructions: Text
  evidence_required: Enum (photo, video, text, location)
  impact_metrics: JSON {co2_saved, water_saved, etc.}
  created_by: UUID (FK to User)
  institution_id: UUID (FK, nullable for global challenges)
  status: Enum (draft, active, archived)
  start_date: Timestamp
  end_date: Timestamp
  created_at: Timestamp
}
```

**Submission Entity**:
```
Submission {
  id: UUID (PK)
  user_id: UUID (FK)
  challenge_id: UUID (FK)
  evidence_type: Enum (photo, video, text)
  evidence_urls: Array<String>
  description: Text
  location: JSON {lat, lng, address}
  submitted_at: Timestamp
  verification_status: Enum (pending, approved, rejected, needs_info)
  verified_by: UUID (FK to User, nullable)
  verified_at: Timestamp
  feedback: Text
  ai_verification_score: Float (0-1)
  fraud_flags: Array<String>
}
```

**Gamification Entities**:
```
UserPoints {
  user_id: UUID (PK, FK)
  total_points: Integer
  level: Integer
  level_progress: Float (0-100)
  eco_coins: Integer
  current_streak: Integer
  longest_streak: Integer
  last_activity_date: Date
  updated_at: Timestamp
}

Badge {
  id: UUID (PK)
  name: String
  description: Text
  icon_url: String
  category: String
  criteria: JSON
  rarity: Enum (common, rare, epic, legendary)
  points_value: Integer
}

UserBadge {
  user_id: UUID (FK)
  badge_id: UUID (FK)
  earned_at: Timestamp
  PRIMARY KEY (user_id, badge_id)
}

Leaderboard {
  id: UUID (PK)
  type: Enum (class, school, district, state, national)
  scope_id: UUID (class_id, institution_id, etc.)
  period: Enum (daily, weekly, monthly, semester, annual)
  start_date: Date
  end_date: Date
  rankings: JSON Array [{user_id, rank, points, change}]
  updated_at: Timestamp
}
```

### Relationships

- User → Profile (1:1)
- User → Institution (N:1)
- User → Submissions (1:N)
- User → UserPoints (1:1)
- User → UserBadges (1:N)
- Challenge → Submissions (1:N)
- Institution → Users (1:N)
- Institution → Challenges (1:N)
- Badge → UserBadges (1:N)

---

## Gamification Engine Design

### Points Calculation Engine

**Base Points Formula**:
```
base_points = challenge.points
multiplier = 1.0

// Streak multiplier
if (user.current_streak >= 7):
  multiplier += min(user.current_streak * 0.1, 0.5)

// First-time bonus
if (is_first_completion(user, challenge)):
  multiplier += 0.25

// Team challenge bonus
if (challenge.is_team_challenge):
  multiplier += 0.20

// Quality bonus (based on verification score)
if (submission.ai_verification_score > 0.8):
  multiplier += 0.15

final_points = base_points * multiplier
```

**Level Progression**:
```
Level thresholds (exponential growth):
Level 1: 0 points
Level 2: 100 points
Level 3: 300 points
Level 4: 600 points
Level 5: 1000 points
...
Level N: (N-1)^2 * 100 points

Current level = floor(sqrt(total_points / 100)) + 1
Progress to next level = (total_points - current_level_threshold) / 
                         (next_level_threshold - current_level_threshold) * 100
```


### Badge Award Engine

**Badge Criteria Evaluation**:
```javascript
// Example: Water Warrior Badge
{
  "badge_id": "water-warrior",
  "criteria": {
    "type": "category_completion",
    "category": "water",
    "min_challenges": 10,
    "min_impact": {"water_saved": 1000} // liters
  }
}

// Evaluation logic
function evaluateBadge(user, badge) {
  const userStats = getUserCategoryStats(user.id, badge.criteria.category);
  
  if (userStats.completed_challenges >= badge.criteria.min_challenges &&
      userStats.water_saved >= badge.criteria.min_impact.water_saved) {
    awardBadge(user.id, badge.id);
    sendNotification(user.id, "badge_earned", badge);
  }
}
```

**Badge Types**:
1. **Milestone Badges**: Triggered by specific achievements (100 challenges, 1000 points)
2. **Category Badges**: Earned by completing challenges in specific categories
3. **Streak Badges**: Awarded for maintaining activity streaks
4. **Impact Badges**: Based on environmental impact metrics
5. **Social Badges**: Earned through collaboration and community engagement
6. **Event Badges**: Limited-time badges for special campaigns

### Leaderboard Update Engine

**Real-Time Leaderboard (Redis)**:
```
// Sorted set structure
Key: leaderboard:{type}:{scope_id}:{period}
Score: total_points
Member: user_id

// Update operation
ZADD leaderboard:school:123:weekly user_456 1250

// Get rankings
ZREVRANGE leaderboard:school:123:weekly 0 99 WITHSCORES

// Get user rank
ZREVRANK leaderboard:school:123:weekly user_456
```

**Leaderboard Update Workflow**:
1. User earns points from verified submission
2. Points added to user_points table (PostgreSQL)
3. Event published to message queue
4. Leaderboard service consumes event
5. Updates Redis sorted sets for all applicable leaderboards
6. Calculates rank changes
7. Sends notifications for significant rank improvements
8. Periodic snapshot to PostgreSQL for historical data

**Fair Play Algorithm**:
```
// Detect suspicious patterns
function detectAnomalies(user_id) {
  const recentActivity = getRecentActivity(user_id, days=7);
  
  // Check for unusual submission rate
  if (recentActivity.submissions_per_day > 20) {
    flagForReview(user_id, "high_submission_rate");
  }
  
  // Check for duplicate evidence
  const duplicates = findDuplicateEvidence(user_id);
  if (duplicates.length > 0) {
    flagForReview(user_id, "duplicate_evidence");
  }
  
  // Check for time-based patterns (bot-like behavior)
  if (hasRegularIntervals(recentActivity.timestamps)) {
    flagForReview(user_id, "bot_pattern");
  }
}
```

---

## Task Verification & Moderation Workflow

### Multi-Layer Verification System

**Layer 1: Automated Pre-Validation**
```
function preValidateSubmission(submission) {
  checks = [];
  
  // File format validation
  if (!isValidFileFormat(submission.evidence_urls)) {
    return reject("Invalid file format");
  }
  
  // File size check
  if (getFileSize(submission.evidence_urls) > MAX_SIZE) {
    return reject("File too large");
  }
  
  // Duplicate detection
  if (isDuplicateSubmission(submission)) {
    return reject("Duplicate submission detected");
  }
  
  // Timestamp validation
  if (!isRecentTimestamp(submission.metadata.timestamp)) {
    return flag("Old timestamp");
  }
  
  return pass();
}
```

**Layer 2: AI-Assisted Verification**
```
function aiVerification(submission, challenge) {
  // Image recognition
  const imageLabels = detectLabels(submission.evidence_urls[0]);
  const relevanceScore = calculateRelevance(imageLabels, challenge.category);
  
  // Content moderation
  const moderationResult = moderateContent(submission.evidence_urls);
  if (moderationResult.inappropriate) {
    return {status: "rejected", reason: "inappropriate_content"};
  }
  
  // Authenticity check
  const authenticityScore = detectManipulation(submission.evidence_urls[0]);
  
  // Location verification (if applicable)
  if (challenge.requires_location) {
    const locationValid = verifyLocation(submission.location, challenge.location_constraints);
  }
  
  return {
    score: (relevanceScore + authenticityScore) / 2,
    confidence: calculateConfidence(),
    suggestion: relevanceScore > 0.7 ? "approve" : "review"
  };
}
```


**Layer 3: Peer Review (Optional)**
```
function peerReviewWorkflow(submission) {
  // Select random peers from same grade
  const reviewers = selectRandomPeers(submission.user_id, count=3);
  
  // Send review requests
  reviewers.forEach(reviewer => {
    createReviewTask(reviewer.id, submission.id);
    sendNotification(reviewer.id, "review_request");
  });
  
  // Collect reviews
  waitForReviews(submission.id, timeout=24_hours);
  
  // Calculate consensus
  const reviews = getReviews(submission.id);
  const approvalRate = reviews.filter(r => r.decision === "approve").length / reviews.length;
  
  if (approvalRate >= 0.67) {
    return {status: "approved", method: "peer_consensus"};
  } else if (approvalRate <= 0.33) {
    return {status: "rejected", method: "peer_consensus"};
  } else {
    return {status: "needs_teacher_review", method: "peer_inconclusive"};
  }
}
```

**Layer 4: Teacher Verification**
```
function teacherVerificationQueue(teacher_id) {
  // Prioritize submissions
  const queue = getSubmissions({
    status: "pending_teacher_review",
    class_id: teacher.class_ids,
    order_by: [
      "ai_score DESC",  // High confidence first
      "submitted_at ASC"  // Older submissions first
    ]
  });
  
  // Smart batching
  const batches = groupSimilarSubmissions(queue);
  
  return batches;
}

function teacherDecision(submission_id, decision, feedback) {
  updateSubmission(submission_id, {
    verification_status: decision,
    verified_by: teacher_id,
    verified_at: now(),
    feedback: feedback
  });
  
  if (decision === "approved") {
    awardPoints(submission.user_id, submission.challenge_id);
    updateLeaderboards(submission.user_id);
    checkBadgeEligibility(submission.user_id);
  }
  
  sendNotification(submission.user_id, "verification_complete", {
    status: decision,
    feedback: feedback
  });
  
  // Update teacher reputation
  updateTeacherStats(teacher_id, decision);
}
```

**Layer 5: Expert Validation (High-Value Tasks)**
```
function expertValidationRequired(submission, challenge) {
  return challenge.points > 500 || 
         challenge.category === "advanced" ||
         submission.claimed_impact > threshold;
}

function routeToExpert(submission) {
  const expert = findAvailableExpert(submission.challenge.category);
  assignExpertReview(expert.id, submission.id);
  sendNotification(expert.id, "expert_review_request");
}
```

### Fraud Detection System

**Pattern Analysis**:
```
function analyzeFraudPatterns(user_id) {
  const patterns = {
    submission_velocity: calculateSubmissionRate(user_id),
    evidence_similarity: detectSimilarEvidence(user_id),
    timing_patterns: analyzeSubmissionTiming(user_id),
    location_consistency: checkLocationPatterns(user_id),
    peer_comparison: compareWithPeers(user_id)
  };
  
  const fraudScore = calculateFraudScore(patterns);
  
  if (fraudScore > 0.8) {
    suspendAccount(user_id, "high_fraud_score");
    notifyAdmin(user_id, fraudScore, patterns);
  } else if (fraudScore > 0.5) {
    flagForManualReview(user_id, patterns);
  }
  
  return fraudScore;
}
```

---

## Security Architecture

### Authentication & Authorization

**JWT-Based Authentication**:
```
Access Token:
- Lifetime: 15 minutes
- Contains: user_id, role, institution_id
- Signed with RS256

Refresh Token:
- Lifetime: 7 days
- Stored in httpOnly cookie
- Rotated on each use
- Revocable via blacklist
```

**Role-Based Access Control (RBAC)**:
```
Roles:
- student: Read own data, submit challenges, view leaderboards
- teacher: All student permissions + verify submissions, manage classes
- admin: All teacher permissions + manage institution, view analytics
- ngo: Create campaigns, view aggregated data
- government: View regional analytics, generate reports

Permissions Matrix:
Resource          | Student | Teacher | Admin | NGO | Government
------------------|---------|---------|-------|-----|------------
Own Profile       | RW      | RW      | RW    | RW  | R
Other Profiles    | R       | R       | RW    | R   | R
Challenges        | R       | RW      | RW    | RW  | R
Submissions       | RW(own) | RW      | RW    | R   | R
Analytics         | R(own)  | R(class)| R(all)| R   | R(region)
```


### Data Security

**Encryption**:
- Data in transit: TLS 1.3
- Data at rest: AES-256 encryption for sensitive fields
- Database encryption: Transparent Data Encryption (TDE)
- File storage: Server-side encryption (SSE-S3)

**Sensitive Data Handling**:
```
Encrypted Fields:
- user.password_hash (bcrypt with salt)
- user.email (encrypted at rest)
- user.phone (encrypted at rest)
- profile.date_of_birth (encrypted at rest)

PII Minimization:
- Collect only necessary data
- Anonymize data for analytics
- Pseudonymize user IDs in logs
- Automatic data retention policies
```

**API Security**:
```
Rate Limiting:
- Anonymous: 10 requests/minute
- Authenticated: 100 requests/minute
- Admin: 1000 requests/minute

Request Validation:
- Input sanitization
- SQL injection prevention
- XSS protection
- CSRF tokens for state-changing operations

Security Headers:
- Content-Security-Policy
- X-Frame-Options: DENY
- X-Content-Type-Options: nosniff
- Strict-Transport-Security
```

### Compliance & Privacy

**Data Protection Compliance**:
```
DPDP Act 2023 (India):
- Explicit consent for data collection
- Right to access personal data
- Right to correction
- Right to erasure ("right to be forgotten")
- Data portability
- Breach notification within 72 hours

FERPA-like Student Privacy:
- Parental consent for users under 18
- Limited data sharing with third parties
- Educational purpose restrictions
- Secure data storage and transmission
```

**Audit Logging**:
```
Logged Events:
- User authentication (login, logout, failed attempts)
- Data access (profile views, report generation)
- Data modifications (profile updates, submissions)
- Administrative actions (user management, configuration changes)
- Security events (suspicious activity, access violations)

Log Retention:
- Security logs: 1 year
- Audit logs: 3 years
- Application logs: 90 days
```

---

## Scalability Strategy

### Horizontal Scaling

**Stateless Services**:
- All microservices designed to be stateless
- Session data stored in Redis (shared state)
- No local file storage (use S3)
- Load balancer distributes traffic across instances

**Auto-Scaling Configuration**:
```
Scaling Triggers:
- CPU utilization > 70%: Scale up
- Memory utilization > 80%: Scale up
- Request queue depth > 100: Scale up
- CPU utilization < 30% for 10 minutes: Scale down

Scaling Limits:
- Min instances: 2 (high availability)
- Max instances: 50 (cost control)
- Scale-up cooldown: 2 minutes
- Scale-down cooldown: 10 minutes
```

### Database Scaling

**PostgreSQL Scaling**:
```
Read Replicas:
- 2 read replicas for read-heavy operations
- Read/write splitting at application layer
- Async replication with <1 second lag

Partitioning:
- submissions table: Partitioned by month
- analytics_events table: Partitioned by week
- Automatic partition creation and archival

Connection Pooling:
- PgBouncer for connection management
- Pool size: 100 connections per instance
- Transaction mode for short-lived connections
```

**MongoDB Scaling**:
```
Sharding Strategy:
- Shard key: institution_id (data locality)
- 3 shards initially, expandable to 10
- Config servers: 3 replicas
- Each shard: 3-node replica set

Indexes:
- Compound indexes for common queries
- TTL indexes for temporary data
- Text indexes for search functionality
```

**Redis Scaling**:
```
Redis Cluster:
- 6 nodes (3 masters, 3 replicas)
- Hash slot distribution
- Automatic failover

Use Cases:
- Session storage (TTL: 7 days)
- Leaderboard cache (sorted sets)
- Rate limiting counters
- Real-time notifications queue
```


### Caching Strategy

**Multi-Layer Caching**:
```
Layer 1: CDN (CloudFront/Cloudflare)
- Static assets: 1 year cache
- Images/videos: 30 days cache
- API responses (public): 5 minutes cache

Layer 2: API Gateway Cache
- GET endpoints: 1-5 minutes cache
- Cache invalidation on data updates
- User-specific cache keys

Layer 3: Application Cache (Redis)
- User profiles: 15 minutes
- Challenge lists: 5 minutes
- Leaderboards: 1 minute (real-time updates)
- Content: 1 hour

Layer 4: Database Query Cache
- PostgreSQL query cache
- MongoDB query result cache
```

**Cache Invalidation**:
```
Strategies:
- Time-based expiration (TTL)
- Event-based invalidation (on data updates)
- Tag-based invalidation (related data groups)
- Manual purge (admin action)

Example:
When user completes challenge:
1. Invalidate user profile cache
2. Invalidate user leaderboard entries
3. Invalidate class leaderboard
4. Invalidate school leaderboard
5. Update Redis leaderboard sorted sets
```

### Message Queue Architecture

**RabbitMQ/AWS SQS**:
```
Queues:
1. verification-queue: Submission verification tasks
2. notification-queue: Notification delivery
3. analytics-queue: Analytics event processing
4. email-queue: Email sending
5. media-processing-queue: Image/video processing

Dead Letter Queues:
- Failed messages after 3 retries
- Manual review and reprocessing
- Alert on DLQ depth > 100

Queue Configuration:
- Message TTL: 24 hours
- Max retries: 3
- Retry delay: Exponential backoff (1m, 5m, 15m)
```

---

## Offline-First Design Strategy

### Progressive Web App (PWA) Architecture

**Service Worker Strategy**:
```javascript
// Cache-first strategy for static assets
self.addEventListener('fetch', (event) => {
  if (isStaticAsset(event.request)) {
    event.respondWith(
      caches.match(event.request)
        .then(response => response || fetch(event.request))
    );
  }
});

// Network-first with cache fallback for API calls
if (isAPICall(event.request)) {
  event.respondWith(
    fetch(event.request)
      .then(response => {
        cache.put(event.request, response.clone());
        return response;
      })
      .catch(() => caches.match(event.request))
  );
}
```

**Offline Data Sync**:
```javascript
// IndexedDB for offline storage
const db = await openDB('eco-platform', 1, {
  upgrade(db) {
    db.createObjectStore('challenges');
    db.createObjectStore('submissions');
    db.createObjectStore('user-data');
    db.createObjectStore('sync-queue');
  }
});

// Queue offline submissions
async function submitChallenge(data) {
  if (navigator.onLine) {
    return await api.post('/submissions', data);
  } else {
    await db.add('sync-queue', {
      type: 'submission',
      data: data,
      timestamp: Date.now()
    });
    return {status: 'queued', offline: true};
  }
}

// Background sync when online
self.addEventListener('sync', (event) => {
  if (event.tag === 'sync-submissions') {
    event.waitUntil(syncQueuedData());
  }
});

async function syncQueuedData() {
  const queue = await db.getAll('sync-queue');
  for (const item of queue) {
    try {
      await api.post('/submissions', item.data);
      await db.delete('sync-queue', item.id);
    } catch (error) {
      console.error('Sync failed:', error);
    }
  }
}
```

**Offline Content Strategy**:
```
Pre-cached Content:
- App shell (HTML, CSS, JS)
- Critical UI assets
- User profile and dashboard data
- Top 10 challenges
- Recent leaderboard snapshot

On-Demand Cache:
- Challenge details when viewed
- Learning content when accessed
- User-generated content (submissions)

Cache Size Management:
- Max cache size: 50MB
- LRU eviction policy
- User control over cached content
```


### Conflict Resolution

**Optimistic Locking**:
```javascript
// Version-based conflict detection
async function updateProfile(userId, updates, version) {
  const result = await db.query(
    'UPDATE profiles SET data = $1, version = version + 1 
     WHERE user_id = $2 AND version = $3',
    [updates, userId, version]
  );
  
  if (result.rowCount === 0) {
    throw new ConflictError('Profile was modified by another device');
  }
}

// Client-side conflict resolution
try {
  await updateProfile(userId, localChanges, localVersion);
} catch (ConflictError) {
  const serverData = await fetchLatestProfile(userId);
  const merged = mergeChanges(localChanges, serverData);
  await updateProfile(userId, merged, serverData.version);
}
```

**Last-Write-Wins for Non-Critical Data**:
```
Applicable to:
- User preferences
- UI settings
- Non-competitive data

Strategy:
- Use timestamp to determine latest change
- Server timestamp is authoritative
- Client accepts server version on conflict
```

---

## API Design Principles

### RESTful API Standards

**Endpoint Structure**:
```
Base URL: https://api.ecoplatform.in/v1

Resource Naming:
- Plural nouns: /users, /challenges, /submissions
- Nested resources: /users/{id}/submissions
- Actions as sub-resources: /submissions/{id}/verify

HTTP Methods:
- GET: Retrieve resources
- POST: Create resources
- PUT: Full update
- PATCH: Partial update
- DELETE: Remove resources

Status Codes:
- 200: Success
- 201: Created
- 204: No content (successful delete)
- 400: Bad request
- 401: Unauthorized
- 403: Forbidden
- 404: Not found
- 409: Conflict
- 422: Validation error
- 429: Rate limit exceeded
- 500: Server error
```

**Request/Response Format**:
```json
// Standard success response
{
  "success": true,
  "data": {
    "id": "123",
    "name": "Water Conservation Challenge"
  },
  "meta": {
    "timestamp": "2026-02-28T10:30:00Z",
    "version": "1.0"
  }
}

// Standard error response
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ]
  },
  "meta": {
    "timestamp": "2026-02-28T10:30:00Z",
    "request_id": "req_abc123"
  }
}

// Paginated response
{
  "success": true,
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 150,
    "total_pages": 8,
    "has_next": true,
    "has_prev": false
  }
}
```

**API Versioning**:
```
Strategy: URL versioning
- /v1/challenges (current)
- /v2/challenges (future)

Deprecation Policy:
- 6 months notice before deprecation
- Support 2 versions simultaneously
- Clear migration documentation
```

### GraphQL API (Optional Phase 2)

**Schema Design**:
```graphql
type User {
  id: ID!
  email: String!
  profile: Profile!
  points: UserPoints!
  badges: [Badge!]!
  submissions(limit: Int, offset: Int): [Submission!]!
}

type Challenge {
  id: ID!
  title: String!
  description: String!
  category: ChallengeCategory!
  difficulty: Difficulty!
  points: Int!
  submissions(userId: ID!): [Submission!]!
}

type Query {
  me: User!
  challenge(id: ID!): Challenge
  challenges(filter: ChallengeFilter, limit: Int, offset: Int): [Challenge!]!
  leaderboard(type: LeaderboardType!, scopeId: ID!): Leaderboard!
}

type Mutation {
  submitChallenge(input: SubmissionInput!): Submission!
  verifySubmission(id: ID!, decision: VerificationDecision!, feedback: String): Submission!
  updateProfile(input: ProfileInput!): Profile!
}

type Subscription {
  leaderboardUpdated(type: LeaderboardType!, scopeId: ID!): Leaderboard!
  notificationReceived(userId: ID!): Notification!
}
```

---

## Technology Stack

### Frontend

**Progressive Web App (PWA)**:
- Core: HTML5, CSS3, Vanilla JavaScript (ES6+)
- Architecture: Offline-first PWA with Service Workers
- UI Framework: Custom CSS3 with lightweight animations
- Game Engine: Phaser.js (2D educational games)
- Animation: CreateJS for interactive content
- Graphics: HTML5 Canvas/SVG for custom visualizations
- Charts: Chart.js for analytics visualization
- Maps: Leaflet (lightweight mapping library)
- Offline Storage: IndexedDB + LocalStorage + Cache API
- Internationalization: i18next (vanilla JS compatible)
- Web App Manifest: Installable app experience

**Why Vanilla JavaScript PWA**:
- Smaller bundle size (<500KB vs 2MB+ for frameworks)
- Faster performance on low-end devices
- Works as web and installable mobile app
- Complete offline functionality
- No app store dependency
- Efficient on low-cost Android phones and laptops
- Direct DOM manipulation for optimal performance


### Backend

**API Services** (Lightweight & Scalable):
- Runtime: Node.js 20 LTS
- Framework: Express.js (minimal, fast)
- Language: JavaScript (ES6+)
- Validation: Joi
- Database Access: pg (PostgreSQL native driver)
- Authentication: JWT (jsonwebtoken library)
- File Upload: Multer
- Task Queue: Simple queue with Redis (optional)
- Cron Jobs: node-cron
- Testing: Jest

**Why Lightweight Backend**:
- Most learning activities work offline
- Backend primarily for progress sync, teacher analytics, content updates
- Minimal server load due to offline-first architecture
- Cost-effective hosting on free/low-cost platforms

**Optional AI/ML** (Phase 2):
- TensorFlow.js (client-side ML for privacy)
- On-device personalization and adaptive learning

### Databases

**Primary Database**:
- PostgreSQL 15 (recommended for analytics and user management)
- Alternative: MySQL 8.0 (lightweight option for smaller deployments)
- Extensions: PostGIS (location data)
- Connection Pooling: pg-pool

**Offline Sync Database** (Optional):
- CouchDB + PouchDB (excellent offline-first synchronization)
- Automatic conflict resolution
- Seamless data merging across devices

**Cache & Session Store** (Optional):
- Redis 7.2 (for high-traffic scenarios)
- Use Cases: Session storage, rate limiting, leaderboard cache

**Client-Side Storage**:
- IndexedDB (lessons, games, progress, assessment scores)
- LocalStorage (user settings, language preferences, UI state)
- Cache API (static assets, games, images, educational media)

### Cloud Infrastructure

**Low-Cost/Free Deployment Options**:

**Frontend (PWA)**:
- Netlify (free tier with global CDN, automatic deployments)
- Vercel (optimized hosting with edge functions)
- GitHub Pages (free static hosting for PWA)
- Cloudflare Pages (free with Cloudflare CDN)

**Backend Services**:
- Render (simple backend hosting with PostgreSQL support)
- Railway (developer-friendly with database integration)
- Fly.io (global application deployment with edge computing)
- Heroku (free/low-cost tier for small deployments)

**Database Hosting**:
- Render PostgreSQL (free tier available)
- Railway PostgreSQL (included with backend)
- Supabase (free PostgreSQL with built-in features)
- ElephantSQL (free PostgreSQL hosting)

**Content Delivery**:
- Cloudflare (free tier CDN for global asset delivery)
- Netlify CDN (included with hosting)
- jsDelivr (free CDN for static assets)

**Optional Cloud Services** (Scale Phase):
- AWS S3 (media storage)
- AWS CloudFront (CDN)
- AWS RDS (managed PostgreSQL)
- DigitalOcean (cost-effective VPS)

### DevOps & CI/CD

**Version Control**:
- Git (GitHub / GitLab)
- Branching: GitFlow
- Code Review: Pull Requests

**CI/CD Pipeline**:
- GitHub Actions (free for public repos)
- Automated testing on push
- Automatic deployment to Netlify/Vercel/Render
- Rollback via Git revert

**Containerization** (Optional):
- Docker (for backend services)
- Docker Compose (local development)

**Monitoring & Observability**:
- Built-in platform monitoring (Netlify/Vercel/Render)
- Sentry (free tier for error tracking)
- Google Analytics (web analytics)
- Custom logging to PostgreSQL
- Uptime monitoring (UptimeRobot free tier)

### Third-Party Integrations

**Authentication**:
- Phone number/School ID login (simple authentication)
- JWT tokens (secure session management)
- Optional OTP via SMS gateway (Twilio/MSG91)

**Communication**:
- Email: Free SMTP or SendGrid free tier
- SMS: Optional via MSG91 (India) or Twilio
- Push Notifications: Web Push API (built into PWA)

**Media Processing**:
- Client-side image compression (browser-native)
- Canvas API for image manipulation
- Optional: TensorFlow.js for image verification

**Analytics**:
- Google Analytics (free web analytics)
- Custom analytics stored in PostgreSQL
- Chart.js for visualization

**Maps & Location**:
- Leaflet with OpenStreetMap (completely free)
- Geolocation API (browser-native)

**AI/ML** (Phase 2):
- TensorFlow.js (client-side ML)
- Web Speech API (voice-based learning)
- On-device personalization for privacy

---

## Deployment Architecture

### Environment Strategy

**Environments**:
1. Development (dev)
2. Staging (staging)
3. Production (prod)

**Environment Configuration**:
```
Development:
- Single instance per service
- Shared database
- Minimal caching
- Debug logging enabled
- Auto-deploy on commit to dev branch

Staging:
- Production-like setup (scaled down)
- Separate database with production-like data
- Full caching enabled
- Production logging
- Auto-deploy on commit to staging branch
- Used for QA and UAT

Production:
- Multi-instance per service (auto-scaling)
- High-availability database setup
- Full caching and CDN
- Production logging and monitoring
- Manual approval for deployment
- Blue-green deployment strategy
```


### Deployment Workflow

**CI/CD Pipeline**:
```
1. Code Commit
   ↓
2. Automated Tests
   - Unit tests
   - Integration tests
   - Linting and code quality checks
   ↓
3. Build Docker Images
   - Tag with commit SHA
   - Push to container registry
   ↓
4. Deploy to Staging
   - Run database migrations
   - Deploy services
   - Run smoke tests
   ↓
5. Manual Approval (Production)
   ↓
6. Deploy to Production
   - Blue-green deployment
   - Health checks
   - Gradual traffic shift
   ↓
7. Post-Deployment
   - Monitor metrics
   - Alert on anomalies
   - Automatic rollback on failure
```

**Blue-Green Deployment**:
```
1. Current production (Blue) serving 100% traffic
2. Deploy new version to Green environment
3. Run health checks on Green
4. Shift 10% traffic to Green (canary)
5. Monitor metrics for 10 minutes
6. If healthy: Shift 50% traffic to Green
7. Monitor for 10 minutes
8. If healthy: Shift 100% traffic to Green
9. Keep Blue running for 1 hour (quick rollback)
10. Decommission Blue environment
```

### Disaster Recovery

**Backup Strategy**:
```
Database Backups:
- Automated daily backups (retained 30 days)
- Weekly backups (retained 3 months)
- Monthly backups (retained 1 year)
- Point-in-time recovery (7 days)

Media Backups:
- S3 versioning enabled
- Cross-region replication
- Lifecycle policies for cost optimization

Configuration Backups:
- Infrastructure as Code in Git
- Secrets backed up in secure vault
```

**Recovery Objectives**:
```
RTO (Recovery Time Objective): 4 hours
RPO (Recovery Point Objective): 1 hour

Recovery Procedures:
1. Detect failure (automated monitoring)
2. Assess impact and root cause
3. Decide: Rollback vs. Fix forward
4. Execute recovery plan
5. Restore from backup if needed
6. Verify system functionality
7. Resume normal operations
8. Post-mortem analysis
```

**High Availability Setup**:
```
Multi-AZ Deployment:
- Services deployed across 3 availability zones
- Database with multi-AZ replication
- Load balancer distributes across AZs
- Automatic failover on AZ failure

Health Checks:
- Application health endpoint: /health
- Database connectivity check
- External dependency checks
- Response time monitoring
- Automatic instance replacement on failure
```

---

## Future Extensibility Considerations

### Modular Architecture

**Plugin System** (Phase 3):
```
Allow institutions to create custom challenges and integrations:
- Challenge templates
- Custom verification rules
- Third-party integrations
- Custom reward systems
- Institutional branding
```

**API Marketplace** (Phase 3):
```
Enable third-party developers to build on the platform:
- Public API with OAuth 2.0
- Developer portal with documentation
- Sandbox environment
- Rate limiting and quotas
- Revenue sharing model
```

### AI/ML Enhancements

**Client-Side AI with TensorFlow.js** (Phase 2):
```
- On-device machine learning for privacy
- Personalized challenge recommendations
- Adaptive difficulty based on user performance
- Image verification (client-side processing)
- Offline ML inference
- No data sent to external servers
```

**Advanced Features** (Phase 2):
```
- Web Speech API for voice-based learning
- Speech recognition for accessibility
- Text-to-speech for content delivery
- Client-side image classification
- Pattern recognition for fraud detection
- Sentiment analysis (lightweight models)
```

**Why Client-Side ML**:
```
- Privacy-first approach (data stays on device)
- Works offline
- No server costs for ML inference
- Faster response times
- Suitable for low-end devices with optimized models
```

### Gamification Evolution

**Phaser.js Game Integration** (Phase 1-2):
```
- 2D educational mini-games
- Interactive environmental simulations
- Puzzle games for learning concepts
- Timed challenges and quizzes
- Multiplayer classroom competitions
- Achievement animations
- Custom game levels based on curriculum
```

**Advanced Features** (Phase 2-3):
```
- CreateJS animations for interactive content
- Canvas-based visualizations
- Peer-to-peer local multiplayer
- Social features (clubs, teams, tournaments)
- Live events and competitions
- Offline game progress sync
```

**Why Phaser.js**:
```
- Lightweight and performant
- Works offline
- Runs on low-end devices
- Rich ecosystem for educational games
- HTML5 Canvas-based (no plugins needed)
- Mobile-friendly touch controls
```

### Integration Expansion

**Educational Platforms** (Phase 2):
```
- Simple REST API for LMS integration
- CSV export/import for Student Information Systems
- Webhook support for real-time updates
- Standard authentication protocols
```

**Environmental Organizations** (Phase 2):
```
- Carbon footprint calculator (client-side)
- Simple API for data sharing
- CSV/JSON export for reporting
- Public API for verified NGOs
```

**Government Systems** (Phase 3):
```
- Read-only API for government dashboards
- Aggregated data export (privacy-compliant)
- Standard reporting formats
- Secure data sharing protocols
```

**Extreme Low Connectivity** (Phase 2):
```
- SMS-based sync for limited internet areas
- Peer-to-peer content sharing via local network
- Bluetooth file transfer for offline content
- Progressive enhancement for 2G networks
```

### Internationalization

**Global Expansion** (Phase 3):
```
- Multi-country support
- Currency localization
- Regional environmental challenges
- International leaderboards
- Compliance with regional regulations
- Partnerships with global NGOs
```

---

## Performance Optimization

### Frontend Optimization

**Vanilla JavaScript Optimization**:
```javascript
// Lazy load modules
async function loadModule(moduleName) {
  const module = await import(`./modules/${moduleName}.js`);
  return module;
}

// Intersection Observer for lazy loading
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      loadContent(entry.target);
    }
  });
});
```

**Image Optimization**:
```
- WebP format with JPEG/PNG fallback
- Responsive images (srcset and sizes)
- Lazy loading with Intersection Observer
- Client-side compression before upload
- Progressive JPEG for faster perceived load
- CDN delivery via Cloudflare
```

**Bundle Optimization**:
```
- Minification with Terser
- Gzip/Brotli compression
- Critical CSS inlining
- Defer non-critical JavaScript
- Code splitting by route
- Target bundle size: <500KB total, <100KB initial load
- No framework overhead
```

**PWA Performance**:
```
- Service Worker caching strategies
- Cache-first for static assets
- Network-first for API calls with cache fallback
- Background sync for offline submissions
- IndexedDB for large data storage
- LocalStorage for quick access data
```

### Backend Optimization

**Database Query Optimization**:
```sql
-- Use indexes for frequent queries
CREATE INDEX idx_submissions_user_status 
ON submissions(user_id, verification_status);

-- Use covering indexes
CREATE INDEX idx_leaderboard_cover 
ON user_points(institution_id, total_points DESC) 
INCLUDE (user_id, level);

-- Optimize N+1 queries with joins
SELECT u.*, p.*, up.total_points 
FROM users u
JOIN profiles p ON u.id = p.user_id
JOIN user_points up ON u.id = up.user_id
WHERE u.institution_id = $1;
```

**API Response Optimization**:
```
- Field selection (return only requested fields)
- Pagination (limit result sets)
- Compression (gzip responses)
- ETags (conditional requests)
- Batch endpoints (reduce round trips)
```

**Caching Strategy**:
```
Cache Hit Rate Target: 80%+

High-value cache targets:
- User profiles (15 min TTL)
- Challenge lists (5 min TTL)
- Leaderboards (1 min TTL)
- Static content (1 day TTL)
- API responses (varies by endpoint)
```

---

## Monitoring & Alerting

### Key Metrics

**Application Metrics**:
```
- Request rate (requests/second)
- Response time (p50, p95, p99)
- Error rate (errors/total requests)
- Availability (uptime percentage)
- Active users (concurrent users)
- Database connections (active/idle)
```

**Business Metrics**:
```
- Daily Active Users (DAU)
- Challenge completion rate
- Verification queue depth
- Average verification time
- User retention rate
- Feature adoption rate
```

**Infrastructure Metrics**:
```
- CPU utilization
- Memory usage
- Disk I/O
- Network throughput
- Database query time
- Cache hit rate
```

### Alerting Rules

**Critical Alerts** (Immediate Response):
```
- Service down (availability < 99%)
- Error rate > 5%
- Response time p95 > 3 seconds
- Database connection pool exhausted
- Disk space > 90%
- Security breach detected
```

**Warning Alerts** (Monitor Closely):
```
- Error rate > 1%
- Response time p95 > 1 second
- CPU utilization > 80%
- Memory usage > 85%
- Cache hit rate < 70%
- Verification queue > 1000
```

**Notification Channels**:
```
- PagerDuty (critical alerts)
- Slack (all alerts)
- Email (daily summary)
- SMS (critical only, on-call engineer)
```