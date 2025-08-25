# Collab Tool Boilerplate (Trello/Asana-like)

Full-stack app with auth, projects, task boards, comments, notifications, and real-time updates via Socket.IO.
Uses **Express + Prisma (SQLite)** on the backend and **React + Vite** on the frontend.

## Quick Start

### 1) Backend
```bash
cd server
cp .env.example .env
npm install
npm run prisma:generate
npm run prisma:migrate
npm run dev
```

This starts the API at `http://localhost:4000`.

### 2) Frontend
```bash
cd ../client
npm install
# Optionally set API url:
# echo "VITE_API_URL=http://localhost:4000" > .env
npm run dev
```

Open the printed URL (default `http://localhost:5173`).

## Features Implemented
- JWT auth (register/login)
- Projects (create, list)
- Tasks (CRUD, status columns: todo/inprogress/done)
- Comments per task
- Notifications stored in DB on task assignment / comment add / task update
- Real-time project rooms via Socket.IO (`join_project`), emitting `task_updated` and `comment_added`
- Simple Kanban drag-and-drop and task modal

## Notes
- DB: SQLite file `server/prisma/dev.db` created by Prisma migrations.
- Role-based access: starter-only (project owner can add members). Extend checks as needed.
- Mentions: Add parsing in comments and create notifications for mentioned users.
- Production: Set strong `JWT_SECRET`, switch to PostgreSQL by changing `provider` + `DATABASE_URL`, run `npm run prisma:deploy`.
- CORS: Server allows all origins by default for dev. Tighten in production.
- Security: Minimal for demo; add rate limiting, input validation (zod), and more granular ACLs.

## API Overview
- `POST /api/auth/register { email, name, password }`
- `POST /api/auth/login { email, password }` -> `{ token, user }`
- `GET /api/projects` -> projects where you are owner/member
- `POST /api/projects { name }`
- `POST /api/projects/:projectId/members { memberEmail, role }`
- `GET /api/tasks/project/:projectId`
- `POST /api/tasks` -> `{ title, projectId, ... }`
- `PUT /api/tasks/:taskId`
- `DELETE /api/tasks/:taskId`
- `GET /api/comments/task/:taskId`
- `POST /api/comments { taskId, content }`

## Roadmap (DIY)
- User mentions in comments (`@name`), server parses & notifies.
- Email or in-app notification tray (`GET /api/notifications` endpoint).
- File attachments on tasks.
- Activity log per project.
