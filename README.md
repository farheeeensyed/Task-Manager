# TaskFlow — Project Management App

A fully self-contained, zero-dependency web app for project and task management with role-based access control.

---

## 🚀 Getting Started

**Option A — Just open in browser:**
```
double-click index.html
```
That's it. No server, no npm, no setup required.

**Option B — Serve locally (optional):**
```bash
npx serve .
# or
python3 -m http.server 3000
```
Then open http://localhost:3000

---

## 🔑 Demo Credentials

| Role   | Email                  | Password   |
|--------|------------------------|------------|
| Admin  | admin@taskflow.io      | admin123   |
| Member | member@taskflow.io     | member123  |
| Member | jamie@taskflow.io      | jamie123   |

---

## ✨ Features

### Authentication
- Signup / Login with email + password
- Role selection (Admin or Member)
- Session persistence across browser refreshes
- Password hashing (Base64 + salt obfuscation)

### Role-Based Access Control (RBAC)
| Feature                | Admin | Member |
|------------------------|-------|--------|
| Create projects        | ✅    | ❌     |
| Edit / delete projects | ✅    | ❌     |
| Create tasks           | ✅    | ✅     |
| Edit own tasks         | ✅    | ✅     |
| Delete any task        | ✅    | ❌     |
| Manage team members    | ✅    | ❌     |
| Change member roles    | ✅    | ❌     |
| View all tasks         | ✅    | ✅     |

### Projects
- Create, edit, delete projects (Admin only)
- Color-coded project cards
- Progress tracking (% tasks done)
- Start date & due date tracking
- Project detail view with task breakdown

### Tasks
- Create and assign tasks to team members
- Priority levels: Low / Medium / High
- Status tracking: To Do → In Progress → In Review → Done
- Due date with overdue detection
- Filter tasks by status or overdue
- Search across all tasks
- Visual indicators for overdue tasks

### Dashboard
- Greeting by time of day
- Stats: total projects, tasks, my tasks, overdue count
- Recent tasks feed
- Live activity log (who did what)

### Team Members (Admin only)
- View all members with task stats
- Promote/demote between Admin ↔ Member
- Remove members
- Invite new members with role assignment

---

## 🗄️ Data Storage

All data is stored in **localStorage** (the browser's built-in key-value store), organized as:

```
tf_users      — user accounts
tf_projects   — projects
tf_tasks      — tasks
tf_activity   — activity log
tf_session    — current session
tf_seeded     — seed data flag
```

### Data Relationships
```
User  ──< Task (assigneeId)
User  ──< Project (ownerId)
Project ──< Task (projectId)
```

### Validation Rules
- Email must be unique across all users
- Password minimum 6 characters
- Task requires a title and a project
- Members cannot create or delete projects
- Members can only edit tasks assigned to them or created by them

---

## 📁 Project Structure

```
taskflow/
├── index.html      ← Entire app (HTML + CSS + JS)
└── README.md       ← This file
```

---

## 🎨 Design System

- **Font**: Cabinet Grotesk (body) + Syne (headings) + DM Mono (code/badges)
- **Theme**: Dark with noise texture overlay
- **Accents**: Purple (#6c63ff), Pink (#ff6584), Green (#43e97b), Orange (#f7971e)
- **Animations**: Page transitions, card hover effects, modal slide-up

---

## 🔧 Extending to a Full Backend

To convert this to a REST API + real database:

1. **Backend**: Node.js + Express
2. **Database**: PostgreSQL or MongoDB
3. **Auth**: Replace localStorage session with JWT (jsonwebtoken)
4. **Password hashing**: Replace Base64 with bcryptjs
5. **API endpoints**:
   - `POST /api/auth/signup`
   - `POST /api/auth/login`
   - `GET/POST /api/projects`
   - `PUT/DELETE /api/projects/:id`
   - `GET/POST /api/tasks`
   - `PUT/DELETE /api/tasks/:id`
   - `GET/PUT/DELETE /api/users/:id`

---

## 📋 REST API Design (Reference)

```
Auth
  POST   /api/auth/signup       Create account
  POST   /api/auth/login        Login, returns JWT

Projects
  GET    /api/projects          List all projects
  POST   /api/projects          Create project [Admin]
  GET    /api/projects/:id      Get project detail
  PUT    /api/projects/:id      Update project [Admin]
  DELETE /api/projects/:id      Delete project [Admin]

Tasks
  GET    /api/tasks             List all tasks (filter by project, assignee, status)
  POST   /api/tasks             Create task
  GET    /api/tasks/:id         Get task detail
  PUT    /api/tasks/:id         Update task
  DELETE /api/tasks/:id         Delete task [Admin or creator]

Users
  GET    /api/users             List all users [Admin]
  GET    /api/users/me          Current user profile
  PUT    /api/users/:id/role    Change role [Admin]
  DELETE /api/users/:id         Remove user [Admin]
```
