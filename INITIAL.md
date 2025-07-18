## 🚀 SAAS MVP TEMPLATE PROJECT:

**⚡ This is a template for creating a SaaS MVP (Minimum Viable Product). You should modify and adapt this template to create your own idea/platform.**

[Insert your SaaS product name and brief description here]

## 📋 CORE FEATURES:

### Authentication & User Management
- [ ] User registration/login (email/password)
- [ ] Password reset functionality
- [ ] User profile management
- [ ] Role-based access control (Admin, User, etc.)

### Main Application Features
[Replace these with your SaaS-specific features. These are just examples:]
- [ ] Feature 1: [Description of your first core feature]
- [ ] Feature 2: [Description of your second core feature]
- [ ] Feature 3: [Description of your third core feature]
- [ ] [Add more features as needed for your specific SaaS]

### Billing & Subscription (Optional for MVP)
- [ ] Stripe integration
- [ ] Subscription plans
- [ ] Payment history
- [ ] Invoice generation

### Admin Dashboard
- [ ] User management
- [ ] Analytics/metrics
- [ ] System settings

## 🏗️ TECHNICAL ARCHITECTURE:

**📌 Note: This architecture is designed as a starting point for your SaaS MVP. Feel free to modify, add, or remove components based on your specific requirements.**

### Frontend (Next.js)
- **Framework**: Next.js 15+ with App Router
- **Styling**: Tailwind CSS
- **State Management**: Zustand or React Context
- **Forms**: React Hook Form + Zod validation
- **API Client**: Axios or Fetch with custom hooks
- **Authentication**: NextAuth.js or custom JWT implementation

### Backend (FastAPI)
- **Framework**: FastAPI 0.115+ with Uvicorn ASGI server
- **Authentication**: JWT with `python-jose` or FastAPI Users
- **Database**: PostgreSQL 16+ with SQLAlchemy 2.0+ ORM
- **Migrations**: Alembic
- **Validation**: Pydantic 2.0+ models
- **Caching**: Redis (optional for MVP)
- **Task Queue**: Celery or Dramatiq (optional for MVP)

### Infrastructure (Docker)
- **Development**: Docker Compose with hot-reload
- **Services**:
  - `frontend`: Next.js (port 3000)
  - `backend`: FastAPI (port 8000)
  - `db`: PostgreSQL (port 5432)
  - `adminer`: Database UI (port 8080)
  - `redis`: Cache/Queue (optional, port 6379)

## 📁 PROJECT STRUCTURE:

```
project-root/
├── frontend/               # Next.js application
│   ├── app/               # App router pages
│   ├── components/        # Reusable components
│   ├── lib/              # Utilities and API client
│   ├── hooks/            # Custom React hooks
│   └── public/           # Static assets
├── backend/               # FastAPI application
│   ├── app/              # Main application package
│   │   ├── __init__.py
│   │   ├── main.py       # FastAPI app instance
│   │   ├── config.py     # Configuration settings
│   │   ├── database.py   # Database connection
│   │   ├── dependencies/ # Shared dependencies
│   │   ├── routers/      # API endpoints
│   │   ├── models/       # SQLAlchemy models
│   │   ├── schemas/      # Pydantic schemas
│   │   └── services/     # Business logic
│   ├── alembic/          # Database migrations
│   ├── tests/            # Test suite
│   └── requirements.txt  # Python dependencies
├── docker-compose.yml     # Docker services
├── .env.example          # Environment variables template
└── README.md             # Project documentation
```

## 🔐 SECURITY CONSIDERATIONS:

- [ ] CORS configuration
- [ ] CSRF protection
- [ ] Rate limiting
- [ ] Input validation
- [ ] SQL injection prevention
- [ ] XSS protection
- [ ] Secure headers
- [ ] Environment variables for secrets

## 📊 DATABASE SCHEMA:

**🔧 Template Schema - Modify according to your domain requirements:**

```
User (Base model - keep and extend as needed)
  - id: UUID
  - email: String (unique)
  - password: String (hashed)
  - created_at: DateTime
  - updated_at: DateTime

[Your Domain Models - Replace with your specific entities]
  - Example: Product, Order, Subscription, etc.
  - Define relationships between entities
  - Add fields specific to your business logic
```

## 🧪 TESTING STRATEGY:

### Backend Testing
- Unit tests for schemas and services
- Integration tests for API endpoints
- Async test support with pytest-asyncio
- Test coverage > 80%

### Frontend Testing
- Component tests with React Testing Library
- Integration tests for API calls
- E2E tests with Playwright (optional)

## 🚢 DEPLOYMENT CONSIDERATIONS:

- [ ] Environment configuration (dev/staging/prod)
- [ ] CI/CD pipeline setup
- [ ] Database migrations strategy
- [ ] Static file serving
- [ ] SSL certificates
- [ ] Monitoring and logging
- [ ] Backup strategy

## 📝 DEVELOPMENT WORKFLOW:

1. **Initial Setup**:
   ```bash
   git clone <repository>
   cp .env.example .env
   docker compose up
   ```

2. **Backend Development**:
   ```bash
   # Create initial migration
   docker compose exec backend alembic init alembic
   docker compose exec backend alembic revision --autogenerate -m "Initial migration"
   docker compose exec backend alembic upgrade head
   
   # Run development server (auto-reload enabled)
   docker compose up backend
   ```

3. **Frontend Development**:
   - Hot reload enabled by default
   - Access at http://localhost:3000

4. **Database Management**:
   - Adminer at http://localhost:8080
   - PostgreSQL at localhost:5432
   
5. **API Documentation**:
   - Swagger UI at http://localhost:8000/docs
   - ReDoc at http://localhost:8000/redoc

## ⚠️ COMMON PITFALLS TO AVOID:

- Not setting up CORS properly between frontend and backend
- Forgetting to add services to Docker network
- Not using environment variables for configuration
- Hardcoding API URLs instead of using environment variables
- Not setting up proper error handling on both frontend and backend
- Forgetting to add volume mounts for hot-reload in development
- Not implementing proper authentication flow from the start

## 🐳 DOCKER COMPOSE EXAMPLE:

```yaml
version: '3.8'

services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile.dev
    volumes:
      - ./frontend:/app
      - /app/node_modules
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_URL=http://localhost:8000
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile.dev
    volumes:
      - ./backend:/app
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/saas_db
      - JWT_SECRET=your-secret-key
      - DEBUG=True
    depends_on:
      - db
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

  db:
    image: postgres:16-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=saas_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  adminer:
    image: adminer:latest
    ports:
      - "8080:8080"
    depends_on:
      - db

volumes:
  postgres_data:
```

## 🎯 GETTING STARTED WITH THIS TEMPLATE:

1. **Clone this template** and rename it to your project
2. **Update this file** (`INITIAL.md`) with your specific:
   - Project name and description
   - Core features for your SaaS
   - Domain models and database schema
   - Any additional technical requirements
3. **Review `CLAUDE.md`** for AI assistant instructions
4. **Start building** your MVP using the provided structure

## 📚 USEFUL RESOURCES:

- FastAPI Documentation: Use Context7 MCP for latest docs
- SQLAlchemy Documentation: Use Context7 MCP for latest docs
- Next.js Documentation: Use Context7 MCP for latest docs
- Docker Compose: Use Context7 MCP for latest docs
- PostgreSQL: Use Context7 MCP for latest docs
- Tailwind CSS: Use Context7 MCP for latest docs
