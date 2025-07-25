## 🏋️ FITPRO MVP TEMPLATE PROJECT:

**💪 FitPro - Sistema de gestión para gimnasios y entrenamiento personal**

FitPro es una plataforma SaaS que permite a los usuarios gestionar su entrenamiento físico, seguir su progreso, acceder a horarios de clases y recibir orientación nutricional personalizada.

## 📋 CORE FEATURES:

### Authentication & User Management (Powered by Clerk)
- [x] User registration/login (email/password, Google, social logins)
- [x] Password reset functionality (manejado por Clerk)
- [x] User profile management (integrado con Clerk)
- [x] Role-based access control (Admin, User, Trainer)
- [x] Multi-factor authentication (MFA)
- [x] Session management

### Main Application Features

#### 1. Página de Inicio
- [x] Dashboard con resumen de actividades recientes
- [x] Métricas rápidas (entrenamientos esta semana, calorías consumidas)
- [x] Accesos directos a funciones principales
- [x] Notificaciones de clases próximas

#### 2. Perfil del Usuario
- [x] Información personal (nombre, edad, altura, peso)
- [x] Objetivos de fitness
- [x] Historial de medidas corporales
- [x] Foto de perfil

#### 3. Horario de Clases
- [x] Calendario de clases disponibles
- [x] Inscripción a clases
- [x] Cancelación de reservas
- [x] Filtros por tipo de clase

#### 4. Seguimiento del Entrenamiento
- [x] Registro de entrenamientos diarios
- [x] Biblioteca de ejercicios
- [x] Seguimiento de progreso (peso levantado, repeticiones)
- [x] Establecimiento y seguimiento de objetivos

#### 5. Nutrición
- [x] Planes de comidas básicos
- [x] Seguimiento de calorías diarias
- [x] Base de datos de alimentos
- [x] Registro de comidas

### Admin Dashboard
- [x] Gestión de usuarios
- [x] Gestión de clases y horarios
- [x] Estadísticas del gimnasio
- [x] Configuración del sistema

## 🏗️ TECHNICAL ARCHITECTURE:

### Frontend (Next.js)
- **Framework**: Next.js 15+ with App Router
- **Styling**: Tailwind CSS
- **State Management**: React Context (simple para MVP)
- **Forms**: React Hook Form + Zod validation
- **API Client**: Fetch with custom hooks
- **Authentication**: Clerk (gestión completa de usuarios)
- **Calendar**: react-big-calendar (compatible y ligero)
- **Charts**: recharts (para gráficas de progreso)

### Backend (FastAPI)
- **Framework**: FastAPI 0.115+ with Uvicorn ASGI server
- **Authentication**: Clerk JWT validation (usando PyJWT)
- **Database**: PostgreSQL 16+ with SQLAlchemy 2.0+ ORM
- **Migrations**: Alembic
- **Validation**: Pydantic 2.0+ models
- **CORS**: Configurado para desarrollo local
- **Clerk SDK**: Para validar tokens y sincronizar usuarios

### Infrastructure (Docker)
- **Development**: Docker Compose with hot-reload
- **Services**:
  - `frontend`: Next.js (port 3000)
  - `backend`: FastAPI (port 8000)
  - `db`: PostgreSQL (port 5432)
  - `adminer`: Database UI (port 8080)

## 📁 PROJECT STRUCTURE:

```
fitpro-mvp/
├── frontend/               # Next.js application
│   ├── app/               # App router pages
│   │   ├── (auth)/        # Clerk auth pages
│   │   │   ├── sign-in/   # Custom sign-in page
│   │   │   ├── sign-up/   # Custom sign-up page
│   │   │   └── onboarding/ # First-time user setup
│   │   ├── dashboard/     # Main dashboard
│   │   ├── profile/       # User profile
│   │   ├── classes/       # Class schedule
│   │   ├── workouts/      # Workout tracking
│   │   └── nutrition/     # Nutrition tracking
│   ├── components/        # Reusable components
│   │   ├── ui/           # UI components
│   │   ├── forms/        # Form components
│   │   └── charts/       # Chart components
│   ├── lib/              # Utilities and API client
│   ├── hooks/            # Custom React hooks
│   ├── middleware.ts     # Clerk auth middleware
│   └── public/           # Static assets
├── backend/               # FastAPI application
│   ├── app/              # Main application package
│   │   ├── __init__.py
│   │   ├── main.py       # FastAPI app instance
│   │   ├── config.py     # Configuration settings
│   │   ├── database.py   # Database connection
│   │   ├── auth.py       # Clerk JWT validation
│   │   ├── dependencies/ # Shared dependencies
│   │   ├── routers/      # API endpoints
│   │   │   ├── clerk.py  # Clerk webhooks
│   │   │   ├── users.py  # User management
│   │   │   ├── classes.py # Class schedule
│   │   │   ├── workouts.py # Workout tracking
│   │   │   └── nutrition.py # Nutrition endpoints
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

- [x] CORS configuration for localhost development
- [x] Clerk JWT validation en cada request
- [x] Autenticación manejada completamente por Clerk
- [x] Webhooks de Clerk para sincronizar usuarios
- [x] Input validation on all endpoints
- [x] SQL injection prevention via SQLAlchemy ORM
- [x] Environment variables for all secrets (Clerk keys)
- [x] HTTPS in production (requerido por Clerk)

## 📊 DATABASE SCHEMA:

```
User (Sincronizado con Clerk)
  - id: UUID (primary key)
  - clerk_user_id: String (unique, not null) # ID de Clerk
  - email: String (unique, not null)
  - role: Enum (user, trainer, admin)
  - created_at: DateTime
  - updated_at: DateTime

UserProfile
  - id: UUID (primary key)
  - user_id: UUID (foreign key -> User)
  - full_name: String # Sincronizado desde Clerk
  - birth_date: Date
  - height: Float (cm)
  - weight: Float (kg)
  - fitness_goals: Text
  - profile_picture: String (URL) # Puede venir de Clerk

GymClass
  - id: UUID (primary key)
  - name: String (not null)
  - description: Text
  - instructor_name: String
  - capacity: Integer
  - duration_minutes: Integer
  - class_type: String (yoga, spinning, etc.)

ClassSchedule
  - id: UUID (primary key)
  - class_id: UUID (foreign key -> GymClass)
  - start_time: DateTime
  - end_time: DateTime
  - available_spots: Integer

ClassBooking
  - id: UUID (primary key)
  - user_id: UUID (foreign key -> User)
  - schedule_id: UUID (foreign key -> ClassSchedule)
  - booking_date: DateTime
  - status: Enum (confirmed, cancelled)

Exercise
  - id: UUID (primary key)
  - name: String (not null)
  - category: String (cardio, strength, flexibility)
  - muscle_group: String

Workout
  - id: UUID (primary key)
  - user_id: UUID (foreign key -> User)
  - date: Date
  - notes: Text

WorkoutExercise
  - id: UUID (primary key)
  - workout_id: UUID (foreign key -> Workout)
  - exercise_id: UUID (foreign key -> Exercise)
  - sets: Integer
  - reps: Integer
  - weight_kg: Float
  - duration_minutes: Integer

Food
  - id: UUID (primary key)
  - name: String (not null)
  - calories_per_100g: Float
  - protein_g: Float
  - carbs_g: Float
  - fat_g: Float

MealPlan
  - id: UUID (primary key)
  - name: String
  - description: Text
  - total_calories: Integer

NutritionLog
  - id: UUID (primary key)
  - user_id: UUID (foreign key -> User)
  - date: Date
  - meal_type: Enum (breakfast, lunch, dinner, snack)
  - food_id: UUID (foreign key -> Food)
  - quantity_g: Float
  - calories: Float
```

## 🧪 TESTING STRATEGY:

### Backend Testing
- Unit tests para autenticación y servicios core
- Tests de integración para endpoints principales
- Fixtures para datos de prueba
- pytest con coverage mínimo 70%

### Frontend Testing
- Tests básicos de componentes críticos
- Tests de formularios con validación
- Mock de API calls

### Testing de Autenticación
```bash
# 1. Obtener token de Clerk (desde el frontend)
const token = await clerk.session.getToken()

# 2. Hacer request al backend
curl -H "Authorization: Bearer $TOKEN" http://localhost:8000/api/users/me
```

## 🚢 DEPLOYMENT CONSIDERATIONS:

- [x] Variables de entorno separadas por ambiente
- [x] Migrations automáticas en desarrollo
- [x] Logs básicos para debugging
- [x] Webhooks de Clerk configurados
- [ ] CI/CD (post-MVP)
- [ ] SSL certificates (post-MVP)
- [ ] Monitoring (post-MVP)

## 📝 DEVELOPMENT WORKFLOW:

1. **Configurar Clerk**:
   ```bash
   # 1. Crear cuenta en https://clerk.com
   # 2. Crear una nueva aplicación "FitPro"
   # 3. Obtener las keys desde el dashboard de Clerk
   # 4. Configurar los métodos de autenticación deseados
   ```

2. **Initial Setup**:
   ```bash
   git clone <repository>
   cp .env.example .env
   # Editar .env con las keys de Clerk
   docker compose up --build
   ```

3. **Backend Development**:
   ```bash
   # Crear migración inicial
   docker compose exec backend alembic revision --autogenerate -m "Initial models"
   docker compose exec backend alembic upgrade head
   
   # Cargar datos de ejemplo (ejercicios, alimentos)
   docker compose exec backend python -m app.seed_data
   ```

4. **Frontend Development**:
   - Hot reload habilitado
   - Acceso en http://localhost:3000
   - Login manejado por Clerk UI

5. **Database Management**:
   - Adminer en http://localhost:8080
   - Server: db
   - Username: postgres
   - Password: postgres
   - Database: fitpro_db
   
6. **API Documentation**:
   - Swagger UI en http://localhost:8000/docs
   - ReDoc en http://localhost:8000/redoc
   - Headers de autorización con Bearer token de Clerk

## ⚠️ COMMON PITFALLS TO AVOID:

- No olvidar configurar CORS para permitir requests desde localhost:3000
- Asegurar que las migraciones se ejecuten antes de iniciar el backend
- Verificar que los volúmenes de Docker estén correctamente montados
- No hardcodear URLs de API, usar variables de entorno
- Validar fechas y horarios considerando timezone
- Manejar casos edge en reservas de clases (capacidad, cancelaciones)
- Configurar correctamente las keys de Clerk en ambos servicios
- No olvidar configurar los webhooks de Clerk para sincronización

## 🐳 DOCKER COMPOSE:

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
      - /app/.next
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_URL=http://localhost:8000
      - NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=${CLERK_PUBLISHABLE_KEY}
      - CLERK_SECRET_KEY=${CLERK_SECRET_KEY}
      - NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
      - NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
      - NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/dashboard
      - NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/onboarding
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
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/fitpro_db
      - CLERK_SECRET_KEY=${CLERK_SECRET_KEY}
      - CLERK_PUBLISHABLE_KEY=${CLERK_PUBLISHABLE_KEY}
      - CLERK_JWT_KEY=${CLERK_JWT_KEY}
      - CORS_ORIGINS=http://localhost:3000
      - DEBUG=True
    depends_on:
      - db
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

  db:
    image: postgres:16-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=fitpro_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5

  adminer:
    image: adminer:latest
    ports:
      - "8080:8080"
    depends_on:
      - db

volumes:
  postgres_data:
```

## 🎯 GETTING STARTED:

1. **Crear cuenta en Clerk**:
   - Ir a https://clerk.com y crear cuenta
   - Crear nueva aplicación llamada "FitPro"
   - En Authentication > Email, habilitar email/password
   - Opcionalmente habilitar Google, GitHub, etc.

2. **Configurar Clerk Dashboard**:
   - Copiar PUBLISHABLE KEY y SECRET KEY
   - En "JWT Templates" crear uno nuevo:
     - Name: `fitpro-backend`
     - Claims: agregar `{"role": "{{user.public_metadata.role}}"}`
   - Copiar el JWT public key

3. **Clonar y configurar proyecto**:
   ```bash
   git clone <repository>
   cd fitpro-mvp
   cp .env.example .env
   # Editar .env con las keys de Clerk
   ```

4. **Iniciar servicios**:
   ```bash
   docker compose up --build
   ```

5. **Configurar base de datos**:
   ```bash
   # En otra terminal
   docker compose exec backend alembic upgrade head
   docker compose exec backend python -m app.seed_data
   ```

6. **Configurar Webhooks de Clerk** (para sincronizar usuarios):
   - En Clerk Dashboard > Webhooks
   - Endpoint URL: `http://localhost:8000/api/clerk/webhook`
   - Events: `user.created`, `user.updated`, `user.deleted`

7. **Acceder a la aplicación**:
   - Frontend: http://localhost:3000
   - API Docs: http://localhost:8000/docs
   - Adminer: http://localhost:8080

## 📚 IMPLEMENTATION GUIDE:

### Frontend Implementation

#### Layout Principal (app/layout.tsx)
```typescript
import { ClerkProvider } from '@clerk/nextjs'
import { Inter } from 'next/font/google'
import './globals.css'

const inter = Inter({ subsets: ['latin'] })

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <ClerkProvider>
      <html lang="es">
        <body className={inter.className}>{children}</body>
      </html>
    </ClerkProvider>
  )
}
```

#### Middleware de Autenticación (middleware.ts)
```typescript
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server'

const isProtectedRoute = createRouteMatcher([
  '/dashboard(.*)',
  '/profile(.*)', 
  '/classes(.*)',
  '/workouts(.*)',
  '/nutrition(.*)',
  '/admin(.*)'
])

export default clerkMiddleware(async (auth, req) => {
  if (isProtectedRoute(req)) {
    await auth.protect()
  }
})

export const config = {
  matcher: [
    '/((?!.*\\..*|_next).*)',
    '/', 
    '/(api|trpc)(.*)'
  ]
}
```

#### Hook para API Calls (hooks/useApi.ts)
```typescript
import { useAuth } from '@clerk/nextjs'

export function useApi() {
  const { getToken } = useAuth()

  const fetchWithAuth = async (url: string, options: RequestInit = {}) => {
    const token = await getToken()
    
    const response = await fetch(`${process.env.NEXT_PUBLIC_API_URL}${url}`, {
      ...options,
      headers: {
        ...options.headers,
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json',
      },
    })

    if (!response.ok) {
      throw new Error(`API Error: ${response.statusText}`)
    }

    return response.json()
  }

  return { fetchWithAuth }
}
```

### Backend Implementation

#### Configuración (app/config.py)
```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    DATABASE_URL: str
    CLERK_SECRET_KEY: str
    CLERK_PUBLISHABLE_KEY: str
    CLERK_JWT_KEY: str
    CORS_ORIGINS: list[str] = ["http://localhost:3000"]
    DEBUG: bool = True

    class Config:
        env_file = ".env"

settings = Settings()
```

#### Validación JWT de Clerk (app/auth.py)
```python
from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
import jwt
from app.config import settings

security = HTTPBearer()

async def get_current_user(
    credentials: HTTPAuthorizationCredentials = Depends(security)
) -> dict:
    try:
        payload = jwt.decode(
            credentials.credentials,
            settings.CLERK_JWT_KEY,
            algorithms=["RS256"],
            options={"verify_aud": False}
        )
        return payload
    except jwt.PyJWTError:
        raise HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail="Could not validate credentials"
        )

async def get_current_user_id(
    current_user: dict = Depends(get_current_user)
) -> str:
    return current_user.get("sub")
```

#### Main App Configuration (app/main.py)
```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from app.config import settings
from app.routers import clerk, users, classes, workouts, nutrition

app = FastAPI(title="FitPro API")

# CORS middleware
app.add_middleware(
    CORSMiddleware,
    allow_origins=settings.CORS_ORIGINS,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Include routers
app.include_router(clerk.router, tags=["clerk"])
app.include_router(users.router, tags=["users"])
app.include_router(classes.router, tags=["classes"])
app.include_router(workouts.router, tags=["workouts"])
app.include_router(nutrition.router, tags=["nutrition"])

@app.get("/")
def read_root():
    return {"message": "FitPro API v1.0"}
```

## 📦 DEPENDENCIES:

### Backend (requirements.txt):
```
fastapi==0.115.0
uvicorn[standard]==0.30.6
sqlalchemy==2.0.31
alembic==1.13.2
psycopg2-binary==2.9.9
pydantic==2.8.2
pyjwt==2.9.0
cryptography==42.0.8
httpx==0.27.0
python-multipart==0.0.9
```

### Frontend (package.json):
```json
{
  "dependencies": {
    "next": "15.0.0",
    "react": "19.0.0",
    "react-dom": "19.0.0",
    "@clerk/nextjs": "5.7.1",
    "@clerk/themes": "2.1.35",
    "tailwindcss": "3.4.0",
    "react-hook-form": "7.52.0",
    "zod": "3.23.0",
    "@hookform/resolvers": "3.9.0",
    "react-big-calendar": "1.13.0",
    "recharts": "2.12.0",
    "date-fns": "3.6.0"
  }
}
```

### Archivo .env.example:
```bash
# Clerk Configuration
CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
CLERK_JWT_KEY=... # Obtener desde Clerk Dashboard -> API Keys -> Show JWT Public Key

# API Configuration
NEXT_PUBLIC_API_URL=http://localhost:8000

# Database
DATABASE_URL=postgresql://postgres:postgres@db:5432/fitpro_db

# Development
DEBUG=True
CORS_ORIGINS=http://localhost:3000
```

## 📚 USEFUL RESOURCES:

- Clerk Documentation: https://clerk.com/docs
- FastAPI Documentation: Use Context7 MCP for latest docs
- SQLAlchemy Documentation: Use Context7 MCP for latest docs
- Next.js Documentation: Use Context7 MCP for latest docs
- Docker Compose: Use Context7 MCP for latest docs
- PostgreSQL: Use Context7 MCP for latest docs
- Tailwind CSS: Use Context7 MCP for latest docs