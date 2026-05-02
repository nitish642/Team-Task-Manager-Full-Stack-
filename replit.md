# Team Task Manager — Ops Control

## Overview

A full-stack team task management web app with role-based access control (Admin/Member), built on a pnpm monorepo.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **Frontend**: React + Vite (artifacts/task-manager)
- **API framework**: Express 5 (artifacts/api-server)
- **Database**: PostgreSQL + Drizzle ORM
- **Auth**: Clerk (Replit-managed, whitelabel)
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Key Features

- Signup/Login via Clerk (email + social)
- Project creation and management
- Role-based access: Admin (full control) / Member (view + edit tasks)
- Task creation, assignment, status tracking (Todo / In Progress / In Review / Done)
- Task priority levels (Low / Medium / High / Urgent)
- Kanban-style board per project
- Dashboard: stats, recent activity, overdue tasks, breakdown by status/priority
- My Tasks view: all tasks assigned to the current user across all projects
- Project member management (invite by email, change roles)

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)

## Database Schema

- `users` — Clerk-synced user profiles
- `projects` — Projects with status (active/archived/completed)
- `project_members` — User ↔ Project membership with role (admin/member)
- `tasks` — Tasks with status, priority, assignee, due date
- `activity` — Audit trail of task actions

## Auth

- Clerk whitelabel auth provisioned via Replit
- `CLERK_SECRET_KEY`, `CLERK_PUBLISHABLE_KEY`, `VITE_CLERK_PUBLISHABLE_KEY` auto-set
- Proxy middleware at `/api/__clerk` handles Clerk token forwarding
- User records auto-created in DB on first login via `GET /api/users/me`

## API Routes

All routes under `/api`:
- `GET/PUT /users/me` — current user profile
- `GET/POST /projects` — list / create projects
- `GET/PATCH/DELETE /projects/:id` — project detail / update / delete
- `GET/POST /projects/:id/members` — list / add members
- `PATCH/DELETE /projects/:id/members/:userId` — update / remove member
- `GET/POST /projects/:id/tasks` — list / create tasks
- `GET/PATCH/DELETE /tasks/:id` — task detail / update / delete
- `GET /tasks/my` — current user's assigned tasks
- `GET /dashboard/summary` — stats summary
- `GET /dashboard/recent-activity` — recent activity feed
- `GET /dashboard/overdue-tasks` — overdue tasks

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.
