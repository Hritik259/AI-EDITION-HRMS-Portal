# HRMS Lite — AI Edition v2.0

> A production-grade Human Resource Management System with AI-powered attendance insights, risk scoring, and an HR chatbot.

## Features

### Core
- **Employee Management** — Full CRUD with search, department filter, pagination
- **Attendance Tracking** — Mark present/absent/late/half-day with optional notes
- **Dashboard Analytics** — KPI cards, 7-day attendance chart, weekly summary

### AI-Powered (OpenAI GPT-3.5)
- **Risk Scoring** — Automatically flags employees with low attendance rates
- **Smart Note Generation** — AI writes professional HR notes for attendance records
- **Trend Detection** — Week-over-week attendance trend per employee
- **HR Chatbot** — Ask natural language questions about your team

## Tech Stack

| Layer      | Technology                                   |
|------------|----------------------------------------------|
| Backend    | FastAPI, SQLAlchemy (async), Pydantic v2      |
| Database   | SQLite (dev) / PostgreSQL (prod)             |
| AI         | OpenAI GPT-3.5-turbo                         |
| Frontend   | React 18, Tailwind CSS, Recharts             |
| Routing    | React Router v6                              |
| HTTP       | Axios                                        |

## Project Structure

```
hrms-project/
├── backend/
│   ├── app/
│   │   ├── main.py           # FastAPI app, CORS, lifespan
│   │   ├── config.py         # pydantic-settings
│   │   ├── database.py       # async SQLAlchemy engine
│   │   ├── routers/          # employees, attendance, dashboard, ai
│   │   ├── services/         # business logic layer
│   │   ├── models/           # SQLAlchemy ORM models
│   │   ├── schemas/          # Pydantic request/response schemas
│   │   └── core/             # deps, exceptions
│   ├── requirements.txt
│   └── .env
└── frontend/
    ├── src/
    │   ├── pages/            # Dashboard, Employees, Attendance
    │   ├── components/       # Layout, Modal, StatCard, Toast
    │   └── api/              # axios instance
    ├── package.json
    └── tailwind.config.js
```

## Quick Start

### Prerequisites
- Python 3.11+
- Node.js 18+

### 1. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv

# Activate it
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
# Edit .env and add your OpenAI API key (optional but recommended)
# OPENAI_API_KEY=sk-...

# Start the server
uvicorn app.main:app --reload --port 8000
```

Backend runs at: http://localhost:8000  
API Docs: http://localhost:8000/docs

### 2. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start dev server
npm run dev
```

Frontend runs at: http://localhost:5173

## API Endpoints

### Employees
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET    | /api/employees | List all (supports ?q=search&department=filter) |
| POST   | /api/employees | Create employee |
| GET    | /api/employees/{id} | Get single employee |
| PUT    | /api/employees/{id} | Update employee |
| DELETE | /api/employees/{id} | Delete employee |
| GET    | /api/employees/departments | List all departments |

### Attendance
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET    | /api/attendance | List all records |
| POST   | /api/attendance | Mark attendance |
| PUT    | /api/attendance/{id} | Update record |
| DELETE | /api/attendance/{id} | Delete record |
| GET    | /api/attendance/employee/{id} | Employee's history |

### Dashboard
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET    | /api/dashboard/stats | Full dashboard data |

### AI
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET    | /api/ai/risk | At-risk employees |
| GET    | /api/ai/trend/{id} | Employee trend |
| POST   | /api/ai/generate-note | Smart note generation |
| POST   | /api/ai/query | HR chatbot |

## Production Deployment

### Backend (Render / Railway)
1. Set environment variables: `DATABASE_URL`, `OPENAI_API_KEY`, `FRONTEND_URL`
2. Build command: `pip install -r requirements.txt`
3. Start command: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`

### Frontend (Vercel)
1. Set `VITE_API_URL` to your backend URL
2. Build command: `npm run build`
3. Output directory: `dist`

## Design Decisions

- **Async SQLAlchemy** — Non-blocking DB calls handle concurrent users efficiently
- **Service layer** — Business logic separated from routers for testability
- **SQLite for dev, PostgreSQL for prod** — Zero config locally, scalable in production
- **Rule-based AI fallback** — Works without OpenAI key; AI features enhance when key is present
- **Pydantic v2** — Faster validation with field-level validators
