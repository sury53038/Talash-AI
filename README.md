# 🔍 SafeFind AI — Missing Person Identification System

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-18+-339933?style=for-the-badge&logo=node.js&logoColor=white"/>
  <img src="https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/MongoDB-7.0-47A248?style=for-the-badge&logo=mongodb&logoColor=white"/>
  <img src="https://img.shields.io/badge/Google_Gemini-API-4285F4?style=for-the-badge&logo=google&logoColor=white"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge"/>
</p>

> An AI-powered real-time missing person detection and identification system built for rapid search, alert generation, and law enforcement coordination.

---

## 📌 Table of Contents

- [About the Project](#-about-the-project)
- [System Architecture](#-system-architecture)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [API Reference](#-api-reference)
- [How It Works](#-how-it-works)
- [Screenshots](#-screenshots)
- [Team](#-team)

---

## 🧠 About the Project

SafeFind AI is a full-stack missing person identification system that combines **real-time facial recognition**, **AI-generated alerts**, and a **police command dashboard** into a single platform.

Built during a hackathon, the system allows anyone to upload a photo of an unknown person and instantly cross-reference it against a database of missing persons. When a match is found, Google Gemini AI automatically generates a professional alert message for law enforcement.

**The problem it solves:** India reports thousands of missing persons every year, especially individuals with cognitive impairments, dementia, or autism. Traditional identification processes are slow and manual. SafeFind AI makes identification instant.

---

## 🏗 System Architecture

```
┌─────────────────────────────────────────────────────┐
│                    FRONTEND (HTML/JS)                │
│  index.html  search.html  report.html  dashboard.html│
└─────────────────┬───────────────────────────────────┘
                  │ HTTP / REST API
┌─────────────────▼───────────────────────────────────┐
│              NODE.JS BACKEND (Express)               │
│                  Port: 5000                          │
│                                                      │
│  /api/search     → Search by face descriptor        │
│  /api/persons    → List all records (dashboard)     │
│  /api/person     → Add / Delete / Mark Found        │
│  /api/auth/login → Generate Police JWT token        │
└──────────┬──────────────────┬───────────────────────┘
           │                  │
┌──────────▼──────┐  ┌────────▼────────────────────────┐
│   MONGODB       │  │     PYTHON AI SERVICE (Flask)   │
│  (Database)     │  │         Port: 8000              │
│  Person records │  │  face_recognition library        │
│  Face encodings │  │  /verify → Compare two images   │
│  Photo uploads  │  │  Returns match + confidence %   │
└─────────────────┘  └──────────┬──────────────────────┘
                                │ (on match found)
                     ┌──────────▼──────────────────────┐
                     │      GOOGLE GEMINI API          │
                     │  Generates alert messages        │
                     │  Urgency classification          │
                     │  Professional report text        │
                     └─────────────────────────────────┘
```

---

## ✨ Features

### 🔎 Identity Scanner (`search.html`)
- Upload any photo to scan against the missing persons database
- Animated scanning UI with real-time progress feedback
- Displays full match details — name, age, disability type, guardian contact
- AI-generated alert message shown on match

### 📋 Report Missing Person (`report.html`)
- Multi-step form — Personal Info → Photo Upload → Review & Submit
- Camera capture support (mobile-friendly)
- 2MB file size validation with clear error messages
- Biometric data extracted and stored automatically

### 🚔 Police Dashboard (`dashboard.html`)
- Live stat cards — total missing, found cases, system status
- Full paginated table of all records pulled from the database
- Instant search + filter by name, disability, status
- View full record in a detail modal
- Mark as Found with one click
- Delete records (requires Police JWT)
- Export current view as CSV

### 🤖 AI Alert System
- Google Gemini generates professional alert messages on every match
- Urgency classification — HIGH / MEDIUM / LOW
- One-line summary for quick briefing
- 10-second cooldown prevents API spam for the same person

### 🔐 Police Authentication
- JWT-based login — `POST /api/auth/login` with password `admin123`
- All write operations (add, delete, mark found) are protected
- 24-hour token expiry

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, Tailwind CSS (CDN), Vanilla JavaScript |
| Backend API | Node.js, Express.js |
| AI Service | Python, Flask, face_recognition (dlib) |
| Database | MongoDB, Mongoose ODM |
| AI Alerts | Google Gemini 1.5 Flash API |
| Auth | JSON Web Tokens (JWT) |
| File Upload | Multer |
| HTTP Client | Axios |

---

## 📁 Project Structure

```
Ai identification system/
│
├── Backend/                          ← Node.js REST API
│   ├── controllers/
│   │   ├── authController.js         ← Login, JWT generation
│   │   ├── personController.js       ← Add, list, delete, mark found
│   │   └── searchController.js       ← Face descriptor matching
│   │
│   ├── middleware/
│   │   ├── auth.js                   ← JWT verification middleware
│   │   └── upload.js                 ← Multer file upload config
│   │
│   ├── models/
│   │   └── Person.js                 ← Mongoose schema
│   │
│   ├── routes/
│   │   └── api.js                    ← All route definitions
│   │
│   ├── uploads/                      ← Uploaded person photos (auto-created)
│   ├── server.js                     ← Entry point
│   └── package.json
│
├── AI_Service/                       ← Python face comparison service
│   └── ai_server.py                  ← Flask API for face_recognition
│
└── Frontend/                         ← Static HTML/CSS/JS
    ├── index.html                    ← Landing page
    ├── search.html                   ← Identity scanner
    ├── report.html                   ← Report missing person form
    ├── dashboard.html                ← Police command dashboard
    └── app.js                        ← Shared JS utilities + API calls
```

---

## 🚀 Getting Started

### Prerequisites

Make sure the following are installed on your machine:

- [Node.js](https://nodejs.org/) (v18 or later)
- [Python](https://www.python.org/) (v3.8 or later)
- [MongoDB](https://www.mongodb.com/try/download/community) (Community Edition)
- [CMake](https://cmake.org/) (required to build `dlib` for face_recognition on Windows)

---

### Step 1 — Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/safefind-ai.git
cd safefind-ai
```

---

### Step 2 — Start MongoDB

Open a terminal and run:

```bash
mongod
```

Leave this terminal open. MongoDB must be running at all times.

---

### Step 3 — Set up the Backend (Node.js)

```bash
cd Backend
npm install
```

Create a `.env` file in the `Backend/` folder (or rename `.env.example`):

```env
MONGODB_URI=mongodb://localhost:27017/safefind_ai
JWT_SECRET=safefind_police_secret
PORT=5000
```

Start the backend:

```bash
npm run dev
```

You should see:
```
✅ Connected to MongoDB
🚀 SafeFind Backend Running on http://localhost:5000
```

---

### Step 4 — Set up the AI Service (Python)

Open a **new terminal**:

```bash
cd "AI_Service"
pip install flask face-recognition
```

> **Windows users:** If `face-recognition` fails, install these first:
> ```bash
> pip install cmake
> pip install dlib
> pip install face-recognition
> ```

Start the AI service:

```bash
python ai_server.py
```

You should see:
```
AI Server starting on http://127.0.0.1:8000
```

---

### Step 5 — Open the Frontend

No build step needed. Simply open `Frontend/index.html` in your browser by double-clicking it, or drag it into Chrome/Edge.

---

### Step 6 — Log in to the Police Dashboard

The Police dashboard requires a JWT token. To get one, either:

**Option A** — Use the login API:
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"password": "admin123"}'
```

Copy the returned `token` value.

**Option B** — Generate one manually:
```bash
cd Backend
node -e "const j=require('jsonwebtoken'); console.log(j.sign({role:'Police',name:'Officer Demo'},'safefind_police_secret'));"
```

Then open your browser's DevTools (F12) → Console tab, and run:
```javascript
localStorage.setItem('safefind_token', 'YOUR_TOKEN_HERE')
```

Now refresh `dashboard.html` — all actions will work.

---

## 📡 API Reference

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `POST` | `/api/auth/login` | ❌ | Login with password, returns JWT |
| `GET` | `/api/persons` | ✅ | List all persons (dashboard) |
| `POST` | `/api/search` | ❌ | Search by face descriptor (128-number array) |
| `POST` | `/api/person` | ✅ | Add new missing person record |
| `DELETE` | `/api/person/:id` | ✅ | Delete a person record |
| `PATCH` | `/api/person/:id/found` | ✅ | Mark person as Found |
| `GET` | `/health` | ❌ | Backend health check |

**Python AI Service:**

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `http://localhost:8000/verify` | Compare two face images, returns `{ match, confidence }` |

---

## ⚙️ How It Works

### Face Matching Flow

```
User uploads photo
       ↓
Frontend sends image to Node.js backend
       ↓
Node.js forwards both images to Python AI Service (port 8000)
       ↓
Python uses face_recognition to compute face encodings
       ↓
Euclidean distance calculated between encodings
       ↓
If distance ≤ 0.6 → MATCH (confidence = 1 - distance)
       ↓
Node.js queries MongoDB for matching person details
       ↓
Returns match data + photo URL to frontend
       ↓
Frontend displays result with guardian contact info
```

### Gemini Alert Generation

When a match is found through any detection method, the system calls Google Gemini with a structured prompt containing the person's name, detection location, and confidence score. Gemini returns a professional alert message, one-line summary, and urgency classification (HIGH/MEDIUM/LOW) that are displayed to the user and saved with the detection record.

---

## 🖼 Screenshots

| Page | Description |
|---|---|
| `index.html` | Landing page with Identity Scanner and Report Missing CTAs |
| `search.html` | Upload scanner with real-time animation and match results panel |
| `report.html` | 3-step form — personal info, photo upload, review & submit |
| `dashboard.html` | Police portal with live stats, searchable table, and detail modal |

---

## 🔒 Security Notes

- The Police dashboard password (`admin123`) and JWT secret are hardcoded for **hackathon/demo purposes only**
- Before deploying to production, move credentials to environment variables and use a strong random `JWT_SECRET`
- The `node_modules/` folder is included in this repository for offline convenience — it is recommended to add it to `.gitignore` for production repositories

---

## 👥 Team

Built with ❤️ for the SafeFind AI Hackathon Project.

| Role | Responsibility |
|---|---|
| Full-Stack Developer | Node.js backend, MongoDB, REST API |
| AI/ML Engineer | Python face_recognition service, Gemini integration |
| Frontend Developer | HTML/CSS/JS, Tailwind, dashboard UI |

---

## 📄 License

This project is licensed under the MIT License.

---

<p align="center">
  <strong>SafeFind AI</strong> — Reuniting Families, One Face at a Time 🛡️
</p>
