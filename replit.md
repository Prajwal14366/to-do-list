# TaskFlow - Todo List Application

## Overview

TaskFlow is a modern, single-page todo list application built with a full-stack TypeScript architecture. The application provides a clean, productivity-focused interface with secure user authentication through Replit's OAuth system. It features a light green aesthetic inspired by Notion and Linear, designed to reduce cognitive load while maintaining visual appeal. Users can create, edit, complete, and delete tasks with real-time updates and a responsive user interface.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack:**
- **Framework:** React 18 with TypeScript
- **Routing:** Wouter (lightweight client-side routing)
- **State Management:** TanStack Query v5 for server state and data fetching
- **UI Components:** Radix UI primitives with custom shadcn/ui components
- **Styling:** Tailwind CSS with custom design tokens

**Design System:**
- Custom theme using HSL color variables for light green aesthetic
- Component library following "new-york" shadcn style
- Responsive design with mobile-first approach
- Consistent spacing system using Tailwind's spacing scale

**Key Design Decisions:**
- **Component-based architecture:** Reusable UI components in `client/src/components/ui/`
- **Type-safe data fetching:** Custom query client with error handling for 401 unauthorized responses
- **Optimistic updates:** TanStack Query manages cache invalidation after mutations
- **Single-page application:** All routing handled client-side with conditional rendering based on auth state

### Backend Architecture

**Technology Stack:**
- **Runtime:** Node.js with TypeScript
- **Framework:** Express.js
- **Database ORM:** Drizzle ORM
- **Session Management:** express-session with PostgreSQL store (connect-pg-simple)
- **Authentication:** OpenID Connect (OIDC) via Passport.js

**Server Configuration:**
- **Development mode:** Vite dev server with HMR middleware
- **Production mode:** Pre-built static assets served by Express
- **API Routes:** RESTful endpoints under `/api` prefix

**Key Design Decisions:**
- **Separation of concerns:** Database layer (`storage.ts`), routing (`routes.ts`), and app configuration (`app.ts`) are modular
- **Session-based authentication:** Server-side sessions stored in PostgreSQL for security and scalability
- **Environment-specific builds:** Separate entry points for development (`index-dev.ts`) and production (`index-prod.ts`)
- **Type safety:** Shared schema definitions between client and server via `@shared/schema`

### Data Architecture

**Database:**
- **Type:** PostgreSQL (via Neon serverless)
- **ORM:** Drizzle ORM with type-safe queries
- **Migration Strategy:** Drizzle Kit for schema migrations

**Database Schema:**

1. **sessions table:**
   - Stores express-session data
   - Required for Replit OAuth authentication
   - Automatic expiration handling

2. **users table:**
   - Primary key: UUID (auto-generated)
   - Fields: email, firstName, lastName, profileImageUrl
   - Timestamps: createdAt, updatedAt

3. **todos table:**
   - Primary key: UUID (auto-generated)
   - Foreign key: userId references users.id (cascade delete)
   - Fields: title, description, completed (boolean)
   - Timestamps: createdAt, updatedAt

**Key Design Decisions:**
- **UUID primary keys:** Better for distributed systems and security
- **Cascade deletes:** When a user is deleted, all their todos are automatically removed
- **Drizzle Zod integration:** Schema validation using Zod schemas derived from Drizzle tables
- **Soft vs Hard deletes:** Hard deletes implemented (no soft delete tracking)

### Authentication & Authorization

**Authentication Flow:**
- **Provider:** Replit OAuth (OpenID Connect)
- **Strategy:** Passport.js with openid-client
- **Session storage:** PostgreSQL-backed sessions with 7-day TTL
- **Token refresh:** Automatic token refresh handled by OIDC client

**Authorization:**
- **Middleware:** `isAuthenticated` middleware protects all `/api` routes
- **User isolation:** All todo queries filtered by authenticated user's ID
- **Session security:** HTTP-only cookies, secure flag enabled, 7-day expiration

**Key Design Decisions:**
- **Server-side sessions:** More secure than JWT tokens for this use case
- **User upsert pattern:** Users are created or updated on each login to sync profile data
- **Automatic user context:** Request object augmented with user claims via Passport

### API Structure

**RESTful Endpoints:**

1. **Authentication:**
   - `GET /api/auth/user` - Get current authenticated user
   - `GET /api/login` - Initiate OAuth login
   - `GET /api/logout` - End session

2. **Todos:**
   - `GET /api/todos` - List all todos for authenticated user
   - `POST /api/todos` - Create new todo
   - `PATCH /api/todos/:id` - Update existing todo
   - `DELETE /api/todos/:id` - Delete todo

**Request/Response Patterns:**
- JSON request bodies for POST/PATCH
- Validation using Zod schemas (insertTodoSchema, updateTodoSchema)
- Standard HTTP status codes (200, 201, 400, 401, 500)
- Error responses include descriptive messages

**Key Design Decisions:**
- **User scoping:** All todo operations automatically scoped to authenticated user
- **Input validation:** Zod schemas validate all incoming data before database operations
- **Error handling:** Centralized error responses with appropriate status codes

## External Dependencies

### Third-Party Services

1. **Replit Authentication (OIDC)**
   - OAuth 2.0 / OpenID Connect provider
   - Environment variables: `ISSUER_URL`, `REPL_ID`, `SESSION_SECRET`
   - Provides user identity and profile information

2. **Neon Database**
   - Serverless PostgreSQL database
   - Environment variable: `DATABASE_URL`
   - WebSocket connection for low-latency queries

### Key Libraries

1. **UI & Styling:**
   - Radix UI primitives (accessibility-first components)
   - Tailwind CSS (utility-first styling)
   - class-variance-authority (component variant management)
   - Lucide React (icon library)

2. **Data Management:**
   - TanStack Query (server state management)
   - Drizzle ORM (type-safe database queries)
   - Zod (schema validation)

3. **Authentication:**
   - Passport.js (authentication middleware)
   - openid-client (OIDC implementation)
   - express-session (session management)

4. **Build Tools:**
   - Vite (development server and build tool)
   - esbuild (production server bundling)
   - TypeScript (type checking)

### Development Tools

- Replit-specific Vite plugins for runtime error overlay and dev banner
- Drizzle Kit for database migrations
- React Hook Form with Zod resolvers for form validation