# Maintenance Checklist System

A role-based web application for managing station maintenance checklists. Staff submit shift checklists, supervisors review and approve them, and admins manage all records.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 19, Vite, React Router, Axios |
| Backend | Node.js, Express 5 |
| Database | MongoDB (Mongoose) |
| Auth | JWT (Bearer token) |
| API Docs | Swagger UI (`/api-docs`) |

---

## Project Structure

```
Maintenance-Checklist/
├── client/          # React frontend (Vite)
│   ├── src/
│   │   ├── pages/   # LoginPage, AdminPage, StaffPage, SupervisorPage
│   │   ├── api.js   # Axios instance
│   │   └── App.jsx  # Routes + auth state
│   └── .env         # VITE_API_BASE_URL
└── server/          # Express backend
    ├── src/
    │   ├── config/  # db.js, env.js, swagger.js
    │   ├── controllers/
    │   ├── middlewares/
    │   ├── models/  # Mongoose schemas
    │   ├── routes/
    │   ├── services/
    │   ├── validators/
    │   ├── seed.js
    │   └── seed-data.json   # Edit this to customise seed accounts
    └── .env         # MONGO_URI, JWT_SECRET, etc.
```

---

## Prerequisites

- **Node.js** v18 or later
- **MongoDB** — either:
  - Local: [MongoDB Community](https://www.mongodb.com/try/download/community) running on port `27017`
  - Cloud: [MongoDB Atlas](https://cloud.mongodb.com) free cluster

---

## Backend Setup

### 1. Install dependencies

```bash
cd server
npm install
```

### 2. Configure `.env`

Edit `server/.env`:

```env
PORT=5000
NODE_ENV=development
JWT_SECRET=change-this-to-a-long-secret
JWT_EXPIRES_IN=1d
CLIENT_URL=http://localhost:5173

# Local MongoDB
MONGO_URI=mongodb://localhost:27017/maintenance_checklist_development

# MongoDB Atlas (use this instead for production)
# MONGO_URI=mongodb+srv://<user>:<password>@cluster0.xxxxx.mongodb.net/maintenance_checklist_production?appName=Cluster0
```

> Set `NODE_ENV=production` when using Atlas.

### 3. Seed default accounts

```bash
npm run seed
```

Default accounts are defined in `src/seed-data.json`. Edit that file and re-run `npm run seed` to customise — no code changes needed.

### 4. Start the server

```bash
# Development (auto-restarts on changes)
npm run dev

# Production
npm start
```

Server runs at **http://localhost:5000**

---

## Frontend Setup

### 1. Install dependencies

```bash
cd client
npm install
```

### 2. Configure `.env`

Create or edit `client/.env`:

```env
VITE_API_BASE_URL=http://localhost:5000/api
```

### 3. Start the frontend

```bash
npm run dev
```

Frontend runs at **http://localhost:5173**

---

## Default Accounts

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@example.com | Admin@123 |
| Staff | staff@example.com | Staff@123 |
| Supervisor | supervisor@example.com | Supervisor@123 |

> **Inactive account?** Login returns: *"Your account is inactive. Please contact the administrator."*
> Re-enable via Admin → Users → Edit → Active user checkbox.

---

## API Reference

Interactive docs available at **http://localhost:5000/api-docs**

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/login` | Login, returns JWT token |
| GET | `/api/auth/me` | Get current user |

### Admin
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET/POST | `/api/admin/stations` | List / create stations |
| PUT/DELETE | `/api/admin/stations/:id` | Update / delete station |
| GET/POST | `/api/admin/users` | List / create users |
| PUT/DELETE | `/api/admin/users/:id` | Update / delete user |
| GET/POST | `/api/admin/shifts` | List / create shifts |
| PUT/DELETE | `/api/admin/shifts/:id` | Update / delete shift |
| GET/POST | `/api/admin/templates` | List / create checklist templates |
| PUT/DELETE | `/api/admin/templates/:id` | Update / delete template |
| GET | `/api/admin/reports/checklists` | Submission status report |

### Staff
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/staff/my-shifts/today` | Today's assigned shifts |
| GET | `/api/staff/checklists/:shiftId` | Get checklist for a shift |
| POST | `/api/staff/checklists/:shiftId/submit` | Submit completed checklist |

### Supervisor
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/supervisor/submissions` | List submissions (filter by status) |
| POST | `/api/supervisor/submissions/:id/approve` | Approve a submission |
| POST | `/api/supervisor/submissions/:id/reject` | Reject a submission |
| GET | `/api/supervisor/history` | Shift-wise checklist history |

---

## Testing by Role

### Admin
1. Log in as `admin@example.com`
2. Create Stations, Users, Shifts, and Templates
3. Assign staff and supervisors to shifts
4. View Reports for submission summaries

### Staff
1. Log in as `staff@example.com`
2. View today's assigned shifts
3. Open a checklist → complete items → submit
- Mandatory items must be completed before submitting
- Expired shifts cannot be submitted

### Supervisor
1. Log in as `supervisor@example.com`
2. Review submitted checklists
3. Approve or reject with a comment
- Only `submitted` checklists can be reviewed
- Rejection requires a reason

---

## Troubleshooting

**MongoDB connection error**
- Local: ensure MongoDB service is running
- Atlas: whitelist your IP in Network Access → Add IP Address

**"Invalid email or password"**
- Seed has not been run — execute `cd server && npm run seed`

**"Your account is inactive"**
- The user's `isActive` is `false` — enable via Admin → Users → Edit

**CORS / network errors**
- Verify backend is running on port `5000`
- Check `VITE_API_BASE_URL` in `client/.env`
- Check `CLIENT_URL` in `server/.env` matches the frontend origin

**No shifts showing for staff**
- Create a shift in Admin → Shifts with today's date and assign the staff user

---

## Recommended Startup Order

```
1. Start MongoDB (local) or confirm Atlas is reachable
2. cd server && npm run dev
3. cd client && npm run dev
4. Open http://localhost:5173
```
