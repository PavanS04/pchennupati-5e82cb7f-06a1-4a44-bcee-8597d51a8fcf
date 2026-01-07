# Secure Task Management System  
## Overview  
This repository contains a secure Task Management System built using an Nx monorepo. It provides role‑based access control (RBAC) for managing tasks within a two‑level organizational hierarchy.  
## Monorepo Structure  
- `apps/api` – NestJS backend implementing JWT authentication, RBAC decorators, and CRUD endpoints for tasks.  
- `apps/dashboard` – Angular frontend with a responsive dashboard for creating, editing and deleting tasks, sorting and filtering by category, and authenticating against the backend.  
- `libs/data` – Shared TypeScript interfaces and Data Transfer Objects (DTOs) used across the backend and frontend.  
- `libs/auth` – Reusable RBAC logic, guards and decorators for enforcing roles and permissions across the app.  
## Setup Instructions  
1. Clone the repository and install dependencies:  
   - `npm install` at the root to bootstrap the Nx workspace.  
2. Backend setup:  
   - Copy `.env.example` to `.env` in `apps/api` and configure values such as `JWT_SECRET` and database connection.  
   - Run `nx serve api` to start the NestJS server on `http://localhost:3333`.  
3. Frontend setup:  
   - Run `nx serve dashboard` to start the Angular development server on `http://localhost:4200`.  
4. The backend uses SQLite by default for local development; to switch to PostgreSQL update the TypeORM configuration in `apps/api/src/app/app.module.ts`.  
## Data Model  
- **User** – represents an account that belongs to an organization and has one or more roles.  
- **Organization** – two‑level hierarchy that can contain users and tasks.  
- **Role** – one of `Owner`, `Admin` or `Viewer`, which defines a set of permissions.  
- **Task** – a resource with attributes such as title, description, category and status; tasks belong to organizations.  
## Access Control Implementation  
The RBAC system is implemented with custom decorators and guards in `libs/auth`. Each API route uses decorators to enforce permissions based on the user’s role and organization. JWT tokens are issued on login and must be included in the `Authorization` header of each request.  
## API Endpoints  
- `POST /tasks` – create a new task (requires appropriate permission).  
- `GET /tasks` – list all tasks visible to the current user, scoped by role and organization.  
- `PUT /tasks/:id` – update an existing task (if permitted).  
- `DELETE /tasks/:id` – delete a task (if permitted).  
- `GET /audit-log` – return audit log entries; accessible only by `Owner` and `Admin` roles.  
  
Authentication endpoints include `/login` to authenticate and receive a JWT.  
## Future Considerations  
- Implement refresh tokens and CSRF protection for improved security.  
- Add support for advanced role delegation and hierarchical permission inheritance.  
- Optimise permission checks with caching for large user bases.
