# Maintenance Checklist & Shift Verification System

Role-based full-stack application to manage shift-based maintenance and cleaning checklist operations for station/facility environments.

## Tech Stack

- Frontend: React + Vite
- Backend: Node.js + Express
- Database: PostgreSQL + Prisma ORM
- Documentation: Swagger (OpenAPI 3.0)

## Modules Implemented

### Admin
- Manage stations, users, and shifts
- Create dynamic checklist templates with mandatory fields
- View comprehensive checklist reports

### Cleaning Staff
- View assigned shifts with real-time **Pending/Submitted** status badges
- Fill out and submit checklists with item-level remarks
- Auto-scroll for better UX during checklist completion

### Supervisor
- Detailed verification queue with "View Details" functionality
- Review staff remarks and individual item comments
- Approve or reject submissions with supervisor comments

## Local Setup

### 1) Backend
```bash
cd server
npm install
```
Configure `.env`:
```env
DATABASE_URL="postgresql://postgres:Siranjeevi@123@localhost:5432/maintenance_checklist_system?schema=public"
```
Run migrations and seed:
```bash
npx prisma migrate dev --name init
npm run seed
npm run dev
```

### 2) Frontend
```bash
cd client
npm install
npm run dev
```

## API Documentation
Interactive Swagger UI is available at:
`http://localhost:5000/api-docs`
(Includes request bodies, schemas, and interactive "Try it out" testing)

## Database Schema Summary

Core entities:

- `users`
- `stations`
- `shifts`
- `shift_assignments`
- `checklist_templates`
- `checklist_template_items`
- `checklist_submissions`
- `checklist_submission_items`

Relationships:

- Station has many shifts and templates
- Shift has many assignments and submissions
- Template has many template items
- Submission has many submission items
- Users are mapped by role and assignment type

## Current Completion

- Phase 1: completed
- Phase 2: completed
- Phase 3: completed (backend + frontend dashboards)
- Phase 4: in progress (deployment + demo assets + optional final polish)

## Suggested Final Submission Additions

- Demo video (1-2 min) or 6-8 screenshots
- Public Postman collection export (optional if Swagger link provided)
- Deployment links:
  - Frontend (Vercel/Netlify)
  - Backend (Render/Railway)
  - PostgreSQL host (Railway/Supabase depending on free tier availability)
