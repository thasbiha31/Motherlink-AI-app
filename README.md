# MotherLink AI – Maternal Health Check-in Tracker

A hackathon-ready full-stack maternal-health check-in app. It persists data in SQLite and provides a responsive React dashboard, due-date pregnancy timeline, scheduled check-ups and vaccine reminder highlighting, multilingual symptom check-ins, deterministic risk rules, trusted-contact consent flow (demo notification record), voice input, guidance chatbot, and doctor-discussion summary.

## Architecture

- **Frontend:** React + Vite (`frontend/`)
- **Backend/API:** Node.js + Express (`backend/`)
- **Database:** SQLite via `better-sqlite3` (`backend/motherlink.db`, generated automatically)
- **API:** REST endpoints under `/api`

## Quick start

Prerequisites: Node.js 18+ and npm.

```bash
# Terminal 1
cd backend
cp .env.example .env
npm install
npm run dev

# Terminal 2
cd frontend
npm install
npm run dev
```

Open the Vite URL shown in the second terminal (normally `http://localhost:5173`). The backend runs on `http://localhost:4000`.

## Demo data

On first run, the API creates `motherlink.db` and seeds a profile, upcoming check-ups, vaccination, and one pregnancy insight. Use the **Profile** tab to change the due date, preferred input language, trusted contact, and explicit notification consent.

## Features and safety logic

- Pregnancy week is calculated from the saved estimated due date using a 280-day pregnancy baseline, clamped to weeks 1–42.
- Vaccine appointments show a “reminder: tomorrow” badge exactly one day before the saved appointment date.
- Symptom patterns recognize English, Tamil, and common Thanglish phrases for swelling, bleeding, severe headache, fever, abdominal pain, and reduced fetal movement.
- Bleeding, severe headache, abdominal pain, and reduced fetal movement are **high risk**. Fever and swelling are **medium risk**. High-risk entries show urgent advice and only record a trusted-contact notification when a phone number and explicit consent are saved.
- This is a demo decision-support app, **not a medical device**. It never replaces emergency services or professional maternity care.
- Voice check-in uses the browser Web Speech API; it works best in Chrome/Edge and gracefully reports unsupported browsers.

## API routes

| Method | Route | Purpose |
|---|---|---|
| GET | `/api/dashboard` | Profile, calculated timeline, care tasks, insights, recent symptoms |
| PUT | `/api/profile` | Validated profile and consent update |
| POST | `/api/symptoms` | Symptom analysis and persistence |
| POST | `/api/checkups/:id/complete` | Persist a completed check-up |
| POST | `/api/chat` | Persist chat request and return safe guidance |
| GET | `/api/doctor-summary` | Produce a current summary from saved history |

## Environment variables and API keys

Copy `backend/.env.example` to `backend/.env`.

- `PORT` is optional and defaults to `4000`.
- `OPENAI_API_KEY` and `OPENAI_BASE_URL` are documented placeholders for connecting a production LLM. The included app deliberately uses a safe, offline built-in guidance responder so that the demo works with **no API key**. Do not expose AI-provider keys in the frontend.

## Production notes

For a production deployment, add authentication, encrypted data storage, HTTPS, a verified SMS/WhatsApp provider for consented alerts, audit logs, localization review by Tamil-speaking clinicians, and clinical governance/medical-device assessment.