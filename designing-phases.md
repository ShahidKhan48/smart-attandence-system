# Designing Phases  
## Smart Attendance System Using Face Recognition

<div style="border: 2px solid #1a5276; padding: 0.5rem 1rem; border-radius: 4px; background: #ebf5fb; color: #1a5276; font-weight: bold;">

Project Design Document — Based on `smart-attendence-system-using-face-recognition/smart-attandence-system`

</div>

---

## 1. Identify the Problem

| Question | Answer |
|----------|--------|
| **What problem are you solving?** | Manual attendance is time-consuming, error-prone, and allows proxy attendance. Card-based systems can be shared or stolen. There is no reliable way to ensure the right person is present. |
| **Who are the users?** | Students/Employees (attendees), Admin (dashboard, reports, user management), System (face detection & recognition). |

> **Problem Summary:** Educational institutions and corporate offices need an automated, accurate, and tamper-resistant attendance system that identifies the person physically present using facial recognition and records attendance without manual roll call or cards.

**Key issues addressed:**

- Time consumption (manual roll calls)
- Proxy attendance
- Human error and inconsistencies
- Paper waste and data management
- Security (cards shared/stolen)
- Scalability with larger groups

---

## 2. Define Objectives

### Primary objectives

1. **Develop an automated attendance system** using facial recognition technology.
2. **Eliminate proxy attendance** and ensure authentic presence verification.
3. **Reduce time spent** on attendance marking.
4. **Create a centralized database** for attendance management (MySQL).

### Secondary objectives

- Generate comprehensive attendance reports (today’s report, monthly analytics with percentages).
- Provide real-time attendance monitoring via admin dashboard.
- Implement user-friendly interfaces (Bootstrap 5, responsive).
- Ensure scalability (Docker deployment option).
- Maintain high accuracy in face detection (Haar Cascade / face_recognition) and duplicate-prevention logic.

---

## 3. Requirement Gathering

### 3.1 Functional Requirements

| ID  | Requirement |
|-----|-------------|
| FR1 | Add, Update, Delete student/employee records (admin users page). |
| FR2 | Capture and store face encodings during registration (camera + face_encoding in DB). |
| FR3 | Mark attendance via camera: detect face → match encoding → record attendance. |
| FR4 | Store attendance records (user_id, date, time, status Present/Absent). |
| FR5 | Today’s report: list of present users with check-in time. |
| FR6 | Monthly report: days present, attendance percentage (e.g. 22 working days), color-coded (Green ≥75%, Yellow 50–74%, Red &lt;50%). |
| FR7 | Admin login (session-based), logout, and access control for all admin routes. |
| FR8 | Admin settings: working days per month, clear attendance by date or all, delete all users (with confirmation). |

### 3.2 Non-Functional Requirements

| Type | Requirement |
|------|-------------|
| **Performance** | Face detection and matching within a few seconds; support class-sized groups per session. |
| **Security** | Session-based admin authentication; passwords stored (e.g. hashed in DB). |
| **Speed** | Real-time or near real-time attendance marking. |
| **Usability** | Bootstrap 5 UI, Font Awesome icons, clear navigation (Dashboard, Users, Today’s Report, Monthly Report, Settings). |
| **Reliability** | Works under normal lighting; Docker option for consistent deployment. |

---

## 4. Feasibility Study

| Type | Feasibility | Notes |
|------|-------------|-------|
| **Technical** | Yes | Python, Flask, OpenCV, face_recognition/dlib, MySQL; Haar Cascade for detection; web + camera. |
| **Economic** | Yes | Open-source stack; normal webcam; optional XAMPP or Docker. |
| **Time** | Yes | Phases: setup → DB → registration → attendance → admin dashboard → reports → testing/deployment. |

---

## 5. System Design

### 5.1 System Architecture (High-level)

**Model:** Client–Server (Browser ↔ Flask Server ↔ MySQL). Camera feeds to server (via browser or local capture).

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                        SMART ATTENDANCE SYSTEM                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   ┌──────────────┐     HTTP      ┌──────────────────────────────────────┐   │
│   │   Browser    │ ◄──────────►  │         Flask Application            │   │
│   │  (Client)    │               │  (app.py, routes, session, face_utils) │   │
│   └──────┬───────┘               └───────────────┬──────────────────────┘   │
│          │                                        │                          │
│          │  Webcam / Camera                       │  MySQL Connector         │
│          │  (capture frames)                     │                          │
│          ▼                                        ▼                          │
│   ┌──────────────┐                       ┌──────────────────┐                │
│   │   Camera     │                       │  MySQL Database  │                │
│   │  (VideoCapture)                      │  attendance_system│                │
│   └──────────────┘                       │  users, attendance, admin        │                │
│                                          └──────────────────┘                │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Data Flow Diagram

```text
                    ┌─────────────┐
                    │   Camera    │
                    │ (live feed) │
                    └──────┬──────┘
                           │ frames
                           ▼
                    ┌─────────────┐     face detected     ┌─────────────────┐
                    │   Face      │ ───────────────────► │  Face           │
                    │   Detection │                      │  Recognition   │
                    │ (Haar / MTCNN)                      │ (compare enc.)  │
                    └──────┬──────┘                      └────────┬────────┘
                           │                                       │
                           │ encoding                               │ user_id match
                           ▼                                       ▼
                    ┌─────────────┐     store/read          ┌─────────────────┐
                    │   User      │ ◄──────────────────────► │   Database      │
                    │   Register  │     face_encoding        │   (MySQL)       │
                    └─────────────┘                         └────────┬────────┘
                                                                     │
                    ┌─────────────┐     mark present                 │
                    │  Attendance │ ◄────────────────────────────────┘
                    │  Marking    │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  Reporting  │  (Today’s report, Monthly report)
                    │  Dashboard  │
                    └─────────────┘
```

### 5.3 Database Design (As Implemented)

**Tables:**

| Table | Purpose |
|-------|---------|
| **users** | id (PK), name, email (UNIQUE), roll_number (UNIQUE), face_encoding (TEXT), created_at |
| **attendance** | id (PK), user_id (FK → users.id), date (DATE), time (TIME), status (Present/Absent), created_at |
| **admin** | id (PK), username (UNIQUE), password, created_at |

### 5.4 ER Diagram

```text
                    ┌─────────────────────────┐
                    │         users           │
                    ├─────────────────────────┤
                    │ id          (PK)        │
                    │ name                    │
                    │ email       (UNIQUE)    │
                    │ roll_number (UNIQUE)    │
                    │ face_encoding           │
                    │ created_at              │
                    └────────────┬────────────┘
                                 │
                                 │ 1
                                 │
                                 │      N
                                 ▼
                    ┌─────────────────────────┐
                    │      attendance         │
                    ├─────────────────────────┤
                    │ id          (PK)        │
                    │ user_id     (FK)        │
                    │ date                    │
                    │ time                    │
                    │ status                  │
                    │ created_at              │
                    └─────────────────────────┘

                    ┌─────────────────────────┐
                    │         admin           │
                    ├─────────────────────────┤
                    │ id          (PK)        │
                    │ username    (UNIQUE)    │
                    │ password               │
                    │ created_at              │
                    └─────────────────────────┘
                         (no FK to users)
```

### 5.5 Component / Module Overview

```text
┌────────────────────────────────────────────────────────────────────┐
│                     Flask Application (app.py)                      │
├─────────────┬─────────────┬─────────────┬─────────────┬────────────┤
│   config/   │   utils/    │   app/      │   Routes    │   Session  │
│ database.py │ face_utils  │ templates/  │ /, /register│ admin_    │
│             │ (FaceRecog-  │ base, index │ /mark_      │ logged_in  │
│             │  nitionUtils)│ register,  │ attendance  │            │
│             │             │ attendance  │ /admin/*    │            │
│             │             │ admin_*     │             │            │
└─────────────┴─────────────┴─────────────┴─────────────┴────────────┘
```

### 5.6 Interface Design (Screens)

| Screen | Purpose |
|--------|---------|
| **Home (index)** | Links to Register, Mark Attendance, Admin. |
| **Register** | Form (name, email, roll_number) + capture face → save user + face_encoding. |
| **Mark Attendance** | Camera → detect face → match → insert attendance (date, time, Present). |
| **Admin Login** | Username, password → session → redirect to dashboard. |
| **Admin Dashboard** | Stats (total users, today’s attendance), quick links: Users, Today’s Report, Monthly Report, Settings. |
| **Admin Users** | Table: id, name, email, roll_number, created_at. |
| **Admin Today’s Report** | List: name, roll_number, check-in time for today. |
| **Admin Monthly Report** | Name, roll number, days present, attendance % (e.g. 22 days), color-coded badges. |
| **Admin Settings** | Working days, clear attendance (by date / all), delete all users, logout. |

---

## 6. Modeling (UML)

### 6.1 Use Case Diagram

```text
                              ┌─────────────────────────────────────────────────────┐
                              │        Smart Attendance System (Boundary)           │
                              ├─────────────────────────────────────────────────────┤
                              │                                                      │
    ┌─────────┐               │  ┌──────────────────┐     ┌─────────────────────┐   │
    │  Admin  │───────────────┼──│ Login / Logout    │     │ View All Users      │   │
    └─────────┘               │  └──────────────────┘     └─────────────────────┘   │
            │                  │  ┌──────────────────┐     ┌─────────────────────┐   │
            │                  │  │ Today's Report   │     │ Monthly Report      │   │
            │                  │  └──────────────────┘     └─────────────────────┘   │
            │                  │  ┌──────────────────┐     ┌─────────────────────┐   │
            │                  │  │ Settings         │     │ Clear attendance    │   │
            │                  │  └──────────────────┘     └─────────────────────┘   │
            │                  │                                                      │
    ┌─────────┐               │  ┌──────────────────┐     ┌─────────────────────┐   │
    │ Student/│───────────────┼──│ Register (face)   │     │ Mark Attendance     │   │
    │ Employee│               │  └──────────────────┘     └─────────────────────┘   │
    └─────────┘               │            │                         │              │
                              │            └────────────┬─────────────┘              │
                              │                         ▼                            │
                              │              ┌─────────────────────┐                 │
                              │              │ Face Recognition    │                 │
                              │              │ (System use case)   │                 │
                              │              └─────────────────────┘                 │
                              └─────────────────────────────────────────────────────┘
```

### 6.2 Class Diagram (Main entities)

```text
┌─────────────────────┐       ┌─────────────────────┐       ┌─────────────────────┐
│       User           │       │    Attendance       │       │       Admin         │
├─────────────────────┤       ├─────────────────────┤       ├─────────────────────┤
│ - id: int            │  1   │ - id: int            │       │ - id: int            │
│ - name: string       │  *   │ - user_id: int (FK)  │       │ - username: string   │
│ - email: string      │──────│ - date: date         │       │ - password: string  │
│ - roll_number: string│      │ - time: time         │       │ - created_at        │
│ - face_encoding: text│      │ - status: enum       │       └─────────────────────┘
│ - created_at         │      │ - created_at         │
└─────────────────────┘       └─────────────────────┘

┌─────────────────────┐       ┌─────────────────────┐
│ FaceRecognitionUtils│       │  DatabaseConfig      │
├─────────────────────┤       ├─────────────────────┤
│ - face_cascade       │       │ - host, database    │
│ + capture_face_       │       │ - user, password   │
│   encoding()         │       │ + get_connection() │
│ + detect_faces_in_   │       └─────────────────────┘
│   frame()            │
│ + compare_faces()    │
└─────────────────────┘
```

### 6.3 Sequence Diagram – Mark Attendance

```text
  Student        Browser         Flask (app.py)      face_utils      MySQL
     │               │                   │                 │            │
     │  Open camera   │                   │                 │            │
     │──────────────►│                   │                 │            │
     │               │  POST /mark_      │                 │            │
     │               │  attendance       │                 │            │
     │               │──────────────────►│                 │            │
     │               │  (image/frame)    │  detect_faces   │            │
     │               │                   │────────────────►│            │
     │               │                   │                 │  compare   │
     │               │                   │  get encodings  │  with DB   │
     │               │                   │◄────────────────│            │
     │               │                   │  SELECT users   │            │
     │               │                   │─────────────────────────────►│
     │               │                   │◄─────────────────────────────│
     │               │                   │  INSERT attendance           │
     │               │                   │─────────────────────────────►│
     │               │  success + name   │                 │            │
     │               │◄──────────────────│                 │            │
     │  "Present: X"  │                   │                 │            │
     │◄──────────────│                   │                 │            │
```

### 6.4 Activity Diagram – Attendance Marking Flow

```text
    ┌──────┐
    │ Start│
    └──┬───┘
       │
       ▼
    ┌──────────────┐     No     ┌─────────────────┐
    │ Open camera   │───────────►│ Show "Camera    │
    │ & capture     │            │  not available"  │
    └───────┬───────┘            └─────────────────┘
            │ Yes
            ▼
    ┌──────────────┐
    │ Face         │
    │ detected?    │
    └───────┬──────┘
            │ No (continue capture)
            │ Yes
            ▼
    ┌──────────────┐     No     ┌─────────────────┐
    │ Match in DB?  │───────────►│ Show "Unknown   │
    │ (compare      │            │  face"          │
    │  encodings)   │            └─────────────────┘
    └───────┬───────┘
            │ Yes
            ▼
    ┌──────────────┐     Duplicate?    ┌─────────────────┐
    │ Already      │─────────────────►│ Skip / message   │
    │ marked today?│  Yes             │ "Already marked" │
    └───────┬───────┘                  └─────────────────┘
            │ No
            ▼
    ┌──────────────┐
    │ INSERT       │
    │ attendance   │
    │ Show "Present│
    │  : [Name]"   │
    └───────┬───────┘
            │
            ▼
    ┌──────┐
    │ End  │
    └──────┘
```

---

## 7. Technology Selection

| Component | Choice | Reason |
|-----------|--------|--------|
| **Language** | Python 3.8+ | OpenCV, face_recognition/dlib, Flask ecosystem. |
| **Backend** | Flask | Lightweight, easy routing and session; suitable for college project. |
| **Face detection** | OpenCV (Haar Cascade) / face_recognition (dlib) | Pre-trained models; face_encoding storage. |
| **Database** | MySQL | init.sql, setup_database.py; production-ready. |
| **Frontend** | HTML5, CSS3, JavaScript, Bootstrap 5, Font Awesome | Responsive UI, icons. |
| **Dev/Deploy** | VS Code, XAMPP (MySQL), Git, Docker (optional) | Local dev; Docker for consistent deployment. |
| **Hardware** | Webcam (720p+), 4GB+ RAM | Minimum for real-time capture. |

---

## 8. Implementation (Coding)

- **Structure:** `app.py` (routes), `config/database.py`, `utils/face_utils.py`, `app/templates/`, `app/static/`.
- **Order:** DB setup (init.sql, setup_database.py) → config → face_utils → registration route → mark_attendance route → admin routes (login, dashboard, users, today_report, monthly_report, settings, logout, clear_attendance).
- **Conventions:** Consistent naming, single DB connection helper, session key for admin.

---

## 9. Testing

| Level | What to test |
|-------|----------------------|
| **Unit** | Face encoding generation, compare_faces(), date/time logic, duplicate check. |
| **Integration** | Register → save user + encoding; mark_attendance → insert; admin login → dashboard → reports. |
| **System** | End-to-end: register → mark attendance → view today’s report and monthly report; settings (clear attendance). |
| **User acceptance** | Real camera in classroom; reports correct; admin workflow smooth. |

---

## 10. Deployment

- **Docker:** `docker-compose up -d` (Flask + MySQL containers); see project’s Dockerfile and docker-compose.yml.
- **Manual:** Python venv, `pip install -r requirements.txt`, MySQL running, `python setup_database.py`, `python app.py`; access at `http://localhost:5000`.
- Configure `config/database.py` for host, user, password, database name.

---

## 11. Maintenance

- Fix camera/recognition issues; tune detection parameters (e.g. tolerance).
- Update face encodings when users change (re-register or update record).
- Regular DB backups; optional: export reports (PDF/Excel) as per synopsis.
- Monitor session security and admin password policy.

---

<div style="border: 2px solid #1a5276; padding: 1rem; border-radius: 6px; background: #ebf5fb;">

## Summary

This document is the **designing phases** for the **Smart Attendance System Using Face Recognition**, aligned with the contents of the **smart-attandence-system** folder (synopsis, app.py, init.sql, face_utils, admin dashboard, reports). It covers problem identification, objectives, functional and non-functional requirements, feasibility, system design with **architecture**, **data flow**, **ER**, **component**, **use case**, **class**, **sequence**, and **activity** diagrams, technology selection, implementation, testing, deployment, and maintenance — suitable for college submission.

</div>
