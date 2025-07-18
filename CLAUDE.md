## 📦 SaaS MVP Template Instructions

**This is a template for creating SaaS MVPs. Users should modify and adapt this template to build their own ideas and platforms. These instructions help AI assistants understand how to work with this template.**

### 🔄 Project Awareness & Context
- **Always read `PLANNING.md`** at the start of a new conversation to understand the project's architecture, goals, style, and constraints.
- **Check `TASK.md`** before starting a new task. If the task isn't listed, add it with a brief description and today's date.
- **Use consistent naming conventions, file structure, and architecture patterns** as described in `PLANNING.md`.
- **Use docker-compose** for development environment orchestration.

### 🏗️ SaaS MVP Architecture
**Note: This is the base architecture for the template. Users will extend and modify based on their specific SaaS requirements.**
- **Frontend**: Next.js (TypeScript) in `/frontend` directory
  - Use App Router for routing
  - Use React Server Components where appropriate
  - Use TypeScript for type safety
  - Use Tailwind CSS for styling
  - Use `next/font` for optimized font loading
- **Backend**: FastAPI (Python) in `/backend` directory
  - Use FastAPI with Pydantic for data validation
  - Use CORS middleware for cross-origin requests
  - Use SQLAlchemy as ORM
  - Use Alembic for database migrations
  - Follow FastAPI project structure conventions
- **Database**: PostgreSQL with Adminer for database management
- **Development**: All services orchestrated via `docker-compose.yml`

### 🧱 Code Structure & Modularity
- **Never create a file longer than 500 lines of code.** If a file approaches this limit, refactor by splitting it into modules or helper files.
- **Backend (FastAPI)**:
  - Organize by features/modules in `app/` directory
  - Separate files for:
    - `routers/` - API endpoints grouped by feature
    - `schemas/` - Pydantic models for request/response
    - `models/` - SQLAlchemy database models
    - `services/` - Business logic
    - `dependencies/` - Shared dependencies and utilities
  - Use dependency injection for database sessions, auth, etc.
- **Frontend (Next.js)**:
  - Use feature-based folder structure in `app/` directory
  - Separate components into `components/` directory
  - Use custom hooks in `hooks/` directory
  - Keep API calls in `lib/api/` directory
- **Use environment variables** for configuration (`.env` files)

### 🧪 Testing & Reliability
- **Backend Tests**:
  - Use pytest with `httpx` for async tests
  - Use `pytest-asyncio` for async test support
  - Create tests in `tests/` directory mirroring app structure
  - Test models, schemas, endpoints, and services
  - Use `factory_boy` or custom fixtures for test data
  - Aim for >80% test coverage
- **Frontend Tests**:
  - Use Jest and React Testing Library
  - Create tests alongside components (`.test.tsx` files)
  - Test components, hooks, and utility functions
  - Use `msw` for API mocking
- **Integration Tests**:
  - Test API integration between frontend and backend
  - Use Docker Compose for consistent test environment

### ✅ Task Completion
- **Mark completed tasks in `TASK.md`** immediately after finishing them.
- Add new sub-tasks or TODOs discovered during development to `TASK.md` under a "Discovered During Work" section.
- Document any Docker or deployment-related tasks.

### 📎 Style & Conventions
- **Backend (Python/FastAPI)**:
  - Follow PEP8, use type hints extensively
  - Format with `black` and lint with `ruff`
  - Use Pydantic models for all request/response validation
  - Use async/await for all endpoints and database operations
  - Follow FastAPI best practices and conventions
- **Frontend (TypeScript/Next.js)**:
  - Use ESLint and Prettier for formatting
  - Follow React/Next.js best practices
  - Use TypeScript strictly (no `any` types)
  - Follow component naming conventions (PascalCase)
- **API Design**:
  - RESTful API design
  - Consistent JSON response format
  - Proper HTTP status codes
  - API versioning (e.g., `/api/v1/`)

### 🐳 Docker Development Workflow
- **Always use `docker-compose`** for local development
- Services defined in `docker-compose.yml`:
  - `frontend`: Next.js development server
  - `backend`: FastAPI with Uvicorn (auto-reload enabled)
  - `db`: PostgreSQL database
  - `adminer`: Database management UI
- **Volume mounts** for hot-reloading in development
- **Environment variables** managed via `.env` files
- **Networking**: All services on the same Docker network

### 📚 Documentation & Explainability
- **Update `README.md`** with:
  - Docker setup instructions
  - Environment variable documentation
  - API endpoint documentation
  - Development workflow guides
- **Comment non-obvious code** and ensure everything is understandable to a mid-level developer.
- **API Documentation**: Use FastAPI's automatic OpenAPI/Swagger docs
- **Frontend Documentation**: Use JSDoc for complex components

### 🧠 AI Behavior Rules
**When helping users with this template:**
- **Understand this is a template** that users will customize for their specific SaaS idea
- **Guide users in adapting the template** rather than treating it as a fixed project
- **Never assume missing context. Ask questions if uncertain.**
- **Never hallucinate libraries or functions** – only use known, verified packages.
- **Always confirm file paths and module names** exist before referencing them in code or tests.
- **Never delete or overwrite existing code** unless explicitly instructed to or if part of a task from `TASK.md`.
- **Use Context7 MCP** for checking latest documentation and best practices.
- **Help users adapt the template** to their specific SaaS requirements while maintaining the architectural patterns.
- **Suggest modifications and extensions** based on the user's specific use case.