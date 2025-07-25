# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 🏋️ Project Overview

This is **FitPro**, a comprehensive fitness management SaaS platform built as an MVP template that users can customize for their specific fitness business needs. The application provides user authentication, class scheduling, workout tracking, nutrition management, and admin functionality.

## 🏗️ Architecture & Tech Stack

### Frontend (Next.js 15 + TypeScript)
- **Framework**: Next.js 15 with App Router (`/frontend`)
- **Authentication**: Clerk integration with multiple auth providers
- **Styling**: Tailwind CSS 4
- **Language**: TypeScript (strict mode, no `any` types)
- **Key Features**: Server components, client components, middleware for auth

### Backend (FastAPI + Python)
- **Framework**: FastAPI with async/await (`/backend`) 
- **Database**: PostgreSQL 16 with AsyncPG driver
- **ORM**: SQLAlchemy 2.0+ (async)
- **Migrations**: Alembic
- **Authentication**: Clerk JWT validation
- **API Design**: RESTful with `/api/v1/` versioning

### Development Environment
- **Orchestration**: Docker Compose with hot-reload
- **Services**: frontend (3000), backend (8000), db (5432), adminer (8080)
- **Network**: All services on `fitpro-network` bridge

## 🚀 Common Development Commands

### Docker Development (Primary)
```bash
# Start all services with hot-reload
docker compose up --build

# Start specific service
docker compose up frontend backend

# View logs
docker compose logs -f [service_name]

# Stop all services
docker compose down
```

### Frontend Commands (from `/frontend`)
```bash
npm run dev      # Development server (port 3000)
npm run build    # Production build
npm run start    # Production server
npm run lint     # ESLint check
```

### Backend Commands (from `/backend`)
```bash
# Development server (port 8000)
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

# Database migrations
alembic upgrade head    # Apply migrations
alembic revision --autogenerate -m "message"  # Create migration

# Testing
pytest                  # Run all tests
pytest tests/test_specific.py  # Run specific test
pytest --cov=app        # Run with coverage
```

## 📁 Code Architecture Patterns

### Backend Structure (`/backend/app/`)
```
app/
├── main.py              # FastAPI app, CORS, router registration
├── config.py            # Pydantic settings with env vars
├── database.py          # SQLAlchemy async setup
├── auth.py              # Clerk JWT validation
├── dependencies/        # Shared dependencies (auth, db sessions)
├── models/              # SQLAlchemy models (base.py + feature models)
├── schemas/             # Pydantic request/response models
├── routers/             # API endpoints grouped by feature
└── services/            # Business logic layer
```

### Frontend Structure (`/frontend/src/`)
```
src/
├── app/                 # Next.js App Router pages
│   ├── (auth)/         # Auth group (sign-in, sign-up, onboarding)
│   ├── dashboard/      # Main dashboard
│   ├── classes/        # Class scheduling
│   ├── workouts/       # Workout tracking
│   ├── nutrition/      # Nutrition management
│   ├── profile/        # User profiles
│   └── admin/          # Admin panel
└── lib/
    └── api.ts          # Centralized API client with Clerk auth
```

### API Client Pattern
- Use `ApiClient` class in `/frontend/src/lib/api.ts`
- Server components: `getServerApiClient()` 
- Client components: `createClientApiClient(token)`
- All endpoints defined in `endpoints` object
- Automatic Clerk token injection

### Database Model Pattern
- Inherit from `Base` in `models/base.py`
- Use async SQLAlchemy 2.0+ syntax
- UUID primary keys with `id: Mapped[str]`
- Timestamps: `created_at`, `updated_at`
- Foreign keys use proper relationships

### Authentication Flow
- Clerk handles all auth UI and sessions
- Backend validates JWT tokens in `auth.py:validate_clerk_token()`
- Use `Depends(get_current_user)` for protected endpoints
- Frontend middleware redirects unauthenticated users

## 🔧 Environment Configuration

### Required Environment Variables
```bash
# Clerk Authentication (get from clerk.com dashboard)
CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
CLERK_JWT_KEY=-----BEGIN PUBLIC KEY-----...

# Database
DATABASE_URL=postgresql+asyncpg://postgres:postgres@db:5432/fitpro_db

# Development
DEBUG=True
CORS_ORIGINS=http://localhost:3000
```

### Environment Files
- Root: `.env` (Docker Compose variables)
- Frontend: Uses `NEXT_PUBLIC_*` vars from Docker environment
- Backend: Uses Pydantic settings with `.env` file support

## 📊 Database Schema Overview

### Core Models
- **User**: Clerk user sync with fitness profiles
- **FitnessClass**: Gym classes with instructor, capacity, schedule
- **Workout**: Exercise templates and user workout logs  
- **Exercise**: Individual exercises with sets, reps, weight
- **NutritionPlan**: Meal plans with calorie/macro tracking
- **Food**: Food database with nutritional information

### Key Relationships
- Users can book multiple classes (many-to-many)
- Users have multiple workout logs (one-to-many)
- Workouts contain multiple exercises (one-to-many)
- Nutrition plans contain multiple meals (one-to-many)

## 🧪 Testing Strategy

### Backend Testing (`/backend/tests/`)
- **Framework**: pytest with pytest-asyncio
- **Structure**: Mirror app structure in tests/
- **Database**: Use test database with async fixtures
- **API Testing**: httpx async client for endpoint testing
- **Run**: `pytest` or `pytest --cov=app` for coverage

### Frontend Testing
- **Framework**: Jest + React Testing Library (to be implemented)
- **Location**: Alongside components (`.test.tsx`)
- **API Mocking**: MSW for API endpoint mocking

## 🎨 Code Style & Standards

### Backend (Python)
- **Formatting**: Black + Ruff
- **Type Hints**: Required on all functions
- **Async/Await**: All database operations and API endpoints
- **Pydantic**: All request/response validation
- **Error Handling**: HTTPException with proper status codes

### Frontend (TypeScript) 
- **Linting**: ESLint with Next.js config
- **TypeScript**: Strict mode, no `any` types
- **Components**: PascalCase naming
- **API Calls**: Use centralized ApiClient
- **Styling**: Tailwind CSS utility classes

### API Design Standards
- RESTful endpoints with proper HTTP methods
- Consistent JSON response format
- `/api/v1/` versioning
- OpenAPI/Swagger docs auto-generated
- Error responses include message and field info

## 🔒 Security Considerations

### Authentication
- Never store Clerk secret keys in frontend
- Use middleware for route protection
- Validate JWT tokens on backend for all protected routes
- Implement proper CORS configuration

### Data Protection
- Use Pydantic validation for all inputs
- Sanitize database queries (SQLAlchemy handles this)
- Environment variables for all secrets
- Never commit `.env` files

## 📈 Development Workflow

### Starting New Features
1. Check if feature aligns with fitness/SaaS domain
2. Plan database changes first (create Alembic migration)
3. Implement backend API endpoints with tests
4. Create Pydantic schemas for request/response
5. Build frontend components with proper TypeScript
6. Add API client methods and endpoint definitions
7. Test integration between frontend and backend
8. Update documentation

### Code Quality Checks
```bash
# Backend
cd backend && python -m ruff check .
cd backend && python -m black --check .
cd backend && pytest

# Frontend  
cd frontend && npm run lint
cd frontend && npm run build
```

### Database Migrations
```bash
# Create migration after model changes
cd backend && alembic revision --autogenerate -m "Add new feature"

# Apply migrations
cd backend && alembic upgrade head
```

## 🎯 Fitness Domain Context

This is a **fitness management SaaS template** focused on:
- **Gym/Studio Management**: Class scheduling, member management
- **Personal Training**: Workout tracking, exercise libraries
- **Nutrition**: Meal planning, food logging, macro tracking  
- **User Engagement**: Progress tracking, goal setting
- **Business Operations**: Admin dashboards, role-based access

When implementing new features, consider how they fit into typical fitness business workflows and user journeys.

## 🚫 Important Constraints

- **File Size Limit**: No files > 500 lines (refactor into modules)
- **Docker First**: Always use Docker Compose for development
- **Authentication**: Never bypass Clerk - all auth goes through it
- **Database**: Always use async SQLAlchemy 2.0+ patterns
- **Type Safety**: TypeScript strict mode, Python type hints required
- **API Versioning**: All endpoints must use `/api/v1/` prefix
- **Environment**: All config through environment variables

## 🔄 Template Customization Notes

This is a **template for fitness SaaS platforms**. Users will customize:
- Branding and UI components
- Business-specific features (e.g., membership tiers, payment)
- Workout/class types specific to their niche
- Nutrition requirements for their target audience
- Admin features for their operational needs

Guide users in adapting the template while maintaining the architectural patterns and development practices outlined above.