# 🏗️ System Architecture - Daily AI Hub

## Overview

Daily AI Hub is built on a modern, scalable architecture combining React frontend with Express backend and AI-powered content generation.

---

## 📐 System Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     Client Layer                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  React 19 + TypeScript + Tailwind CSS 4             │   │
│  │  - Home Page                                         │   │
│  │  - Article List                                      │   │
│  │  - Article Detail                                    │   │
│  │  - Search & Filter                                   │   │
│  │  - Admin Dashboard                                   │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                     API Layer (tRPC)                         │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Express.js + tRPC + TypeScript                      │   │
│  │  - /api/trpc/articles.*                              │   │
│  │  - /api/trpc/categories.*                            │   │
│  │  - /api/trpc/search.*                                │   │
│  │  - /api/trpc/recommendations.*                       │   │
│  │  - /api/trpc/admin.*                                 │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                   Business Logic Layer                       │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Drizzle ORM + Database Queries                      │   │
│  │  - Article Service                                   │   │
│  │  - Category Service                                  │   │
│  │  - Search Service                                    │   │
│  │  - Recommendation Engine                             │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                   Data Layer                                 │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  MySQL/TiDB Database                                 │   │
│  │  - Articles Table                                    │   │
│  │  - Categories Table                                  │   │
│  │  - Users Table                                       │   │
│  │  - Preferences Table                                 │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                   External Services                          │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  - Google Gemini API (Content Generation)            │   │
│  │  - S3 Storage (File Storage)                         │   │
│  │  - Umami Analytics (Tracking)                        │   │
│  │  - Manus OAuth (Authentication)                      │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔄 Data Flow

### Content Generation Pipeline

```
1. Research Phase
   ├─ Fetch latest AI news
   ├─ Identify trending topics
   └─ Compile research data

2. Outline Generation
   ├─ Create article structure
   ├─ Define key sections
   └─ Plan content flow

3. Content Writing
   ├─ Generate article body
   ├─ Add examples
   └─ Include citations

4. Quality Review
   ├─ Check accuracy
   ├─ Validate formatting
   └─ Approve for publishing

5. Publishing
   ├─ Save to database
   ├─ Generate metadata
   ├─ Update search index
   └─ Notify subscribers
```

### User Interaction Flow

```
User
  ↓
Frontend (React)
  ├─ Render UI
  ├─ Handle interactions
  └─ Call tRPC procedures
  ↓
Backend (Express + tRPC)
  ├─ Validate request
  ├─ Check authentication
  └─ Execute business logic
  ↓
Database (MySQL)
  ├─ Query data
  ├─ Apply filters
  └─ Return results
  ↓
Frontend (React)
  ├─ Receive data
  ├─ Update state
  └─ Render results
  ↓
User (sees updated content)
```

---

## 📦 Component Architecture

### Frontend Components

```
App
├─ Layout
│  ├─ Header
│  │  ├─ Logo
│  │  ├─ Navigation
│  │  └─ Search Bar
│  ├─ Sidebar
│  │  ├─ Categories
│  │  ├─ Trending
│  │  └─ Recent
│  └─ Footer
├─ Pages
│  ├─ Home
│  │  ├─ Hero Section
│  │  ├─ Featured Articles
│  │  └─ Categories Grid
│  ├─ ArticleList
│  │  ├─ Filter Sidebar
│  │  ├─ Article Cards
│  │  └─ Pagination
│  ├─ ArticleDetail
│  │  ├─ Article Header
│  │  ├─ Article Content
│  │  ├─ Related Articles
│  │  └─ Comments
│  ├─ Search
│  │  ├─ Search Input
│  │  ├─ Results Grid
│  │  └─ Filters
│  └─ AdminDashboard
│     ├─ Statistics
│     ├─ Article Management
│     ├─ Category Management
│     └─ User Management
└─ Shared Components
   ├─ ArticleCard
   ├─ CategoryBadge
   ├─ SearchInput
   ├─ Pagination
   └─ LoadingSpinner
```

### Backend Services

```
Server
├─ Routers
│  ├─ articles.router.ts
│  │  ├─ getAll()
│  │  ├─ getBySlug()
│  │  ├─ getByCategory()
│  │  ├─ create()
│  │  ├─ update()
│  │  └─ delete()
│  ├─ categories.router.ts
│  │  ├─ getAll()
│  │  ├─ getById()
│  │  ├─ create()
│  │  └─ update()
│  ├─ search.router.ts
│  │  ├─ search()
│  │  └─ getFilters()
│  ├─ recommendations.router.ts
│  │  ├─ getForUser()
│  │  └─ getSimilar()
│  └─ admin.router.ts
│     ├─ getStats()
│     ├─ manageArticles()
│     └─ manageUsers()
├─ Services
│  ├─ ArticleService
│  ├─ CategoryService
│  ├─ SearchService
│  ├─ RecommendationEngine
│  └─ ContentGenerator
├─ Database
│  ├─ Migrations
│  ├─ Schema
│  └─ Queries
└─ Middleware
   ├─ Authentication
   ├─ Authorization
   ├─ Validation
   └─ Error Handling
```

---

## 🗄️ Database Schema

### Articles Table

```sql
CREATE TABLE articles (
  id VARCHAR(36) PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  slug VARCHAR(255) UNIQUE NOT NULL,
  content LONGTEXT NOT NULL,
  summary TEXT,
  category_id VARCHAR(36),
  tags JSON,
  author VARCHAR(100),
  source_url VARCHAR(500),
  image_url VARCHAR(500),
  view_count INT DEFAULT 0,
  like_count INT DEFAULT 0,
  published_at DATETIME,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (category_id) REFERENCES categories(id),
  INDEX idx_category (category_id),
  INDEX idx_published (published_at),
  FULLTEXT INDEX ft_content (title, content)
);
```

### Categories Table

```sql
CREATE TABLE categories (
  id VARCHAR(36) PRIMARY KEY,
  name VARCHAR(100) NOT NULL UNIQUE,
  slug VARCHAR(100) UNIQUE NOT NULL,
  description TEXT,
  icon VARCHAR(50),
  color VARCHAR(7),
  article_count INT DEFAULT 0,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### Users Table

```sql
CREATE TABLE users (
  id VARCHAR(36) PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  name VARCHAR(100),
  role ENUM('user', 'admin') DEFAULT 'user',
  preferences JSON,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### User Preferences Table

```sql
CREATE TABLE user_preferences (
  id VARCHAR(36) PRIMARY KEY,
  user_id VARCHAR(36) NOT NULL,
  preferred_categories JSON,
  reading_history JSON,
  saved_articles JSON,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

## 🔐 Security Architecture

### Authentication Flow

```
1. User Login
   ├─ Redirect to OAuth provider
   ├─ User authenticates
   └─ Receive authorization code

2. Token Exchange
   ├─ Exchange code for token
   ├─ Create session
   └─ Set secure cookie

3. Authenticated Requests
   ├─ Include session cookie
   ├─ Verify token validity
   └─ Attach user context

4. Protected Routes
   ├─ Check authentication
   ├─ Verify authorization
   └─ Execute procedure
```

### Authorization Levels

```
Public Routes:
├─ GET /articles
├─ GET /articles/:slug
├─ GET /categories
└─ GET /search

Authenticated Routes:
├─ POST /articles/save
├─ GET /recommendations
└─ PUT /preferences

Admin Routes:
├─ POST /articles/create
├─ PUT /articles/:id
├─ DELETE /articles/:id
├─ PUT /categories/:id
└─ GET /admin/stats
```

---

## 🚀 Performance Optimization

### Frontend Optimization

```
1. Code Splitting
   ├─ Route-based splitting
   ├─ Component lazy loading
   └─ Dynamic imports

2. Image Optimization
   ├─ Responsive images
   ├─ WebP format
   └─ Lazy loading

3. Caching Strategy
   ├─ Browser cache
   ├─ Service worker
   └─ CDN cache

4. Bundle Optimization
   ├─ Tree shaking
   ├─ Minification
   └─ Compression
```

### Backend Optimization

```
1. Database Optimization
   ├─ Query optimization
   ├─ Index creation
   └─ Connection pooling

2. Caching Layer
   ├─ Redis cache
   ├─ Query cache
   └─ API response cache

3. API Optimization
   ├─ Response compression
   ├─ Pagination
   └─ Field selection

4. Load Balancing
   ├─ Multiple instances
   ├─ Request distribution
   └─ Failover handling
```

---

## 📊 Monitoring & Observability

### Metrics Collection

```
Frontend Metrics:
├─ Page load time
├─ Time to interactive
├─ Core Web Vitals
└─ User interactions

Backend Metrics:
├─ API response time
├─ Database query time
├─ Error rate
└─ Request throughput

Infrastructure Metrics:
├─ CPU usage
├─ Memory usage
├─ Disk usage
└─ Network bandwidth
```

### Logging Strategy

```
Log Levels:
├─ DEBUG: Development information
├─ INFO: General information
├─ WARN: Warning messages
├─ ERROR: Error messages
└─ FATAL: Critical errors

Log Destinations:
├─ Console (development)
├─ File system (production)
├─ Centralized logging (ELK)
└─ Error tracking (Sentry)
```

---

## 🔄 Deployment Architecture

### CI/CD Pipeline

```
1. Code Push
   ├─ Trigger GitHub Actions
   └─ Run tests

2. Build Phase
   ├─ Install dependencies
   ├─ Run linter
   ├─ Build application
   └─ Run tests

3. Deployment Phase
   ├─ Build Docker image
   ├─ Push to registry
   ├─ Deploy to Vercel
   └─ Run smoke tests

4. Post-Deployment
   ├─ Health checks
   ├─ Performance tests
   └─ Notify team
```

---

## 📈 Scalability Considerations

### Horizontal Scaling

```
Load Balancer
├─ Server Instance 1
├─ Server Instance 2
├─ Server Instance 3
└─ Server Instance N

Shared Resources:
├─ Database (MySQL Cluster)
├─ Cache (Redis Cluster)
├─ File Storage (S3)
└─ CDN (CloudFront)
```

### Vertical Scaling

```
Increase Resources:
├─ CPU cores
├─ Memory (RAM)
├─ Storage capacity
└─ Network bandwidth
```

---

## 🛠️ Technology Choices

| Layer | Technology | Reason |
|-------|-----------|--------|
| Frontend | React 19 | Modern, performant, large ecosystem |
| Styling | Tailwind CSS 4 | Utility-first, fast development |
| Backend | Express.js | Lightweight, flexible, popular |
| Type Safety | TypeScript | Catch errors early, better DX |
| API | tRPC | Type-safe, end-to-end type checking |
| Database | MySQL/TiDB | Reliable, scalable, SQL-based |
| ORM | Drizzle | Type-safe, modern, lightweight |
| AI/LLM | Google Gemini | Powerful, cost-effective, reliable |
| Deployment | Vercel | Serverless, automatic scaling, free tier |
| Hosting | Vercel | Edge functions, automatic deployments |
| Analytics | Umami | Privacy-focused, self-hosted option |

---

**Last Updated**: December 7, 2024
**Maintained by**: Manus AI Team
