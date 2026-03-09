# Database Schema Screenshot Guide (Docker)

## Step 1: Start phpMyAdmin (with your existing containers)

Open terminal in this folder and run:

```bash
docker compose up -d phpmyadmin
```

(This starts only the phpMyAdmin container; MySQL and web are already running.)

---

## Step 2: Open phpMyAdmin in browser

- **URL:** http://localhost:8080

---

## Step 3: Login to database

You can use **either** of these:

| Login type | Username | Password |
|------------|----------|----------|
| **Root**   | `root`   | `rootpassword` |
| **App user** | `appuser` | `apppassword` |

- Click **Go** after entering username and password.

---

## Step 4: Select database

- In the left sidebar, click **`attendance_system`**.

---

## Step 5: Take screenshot for report

You need to show **tables and their structure**:

1. You should see 3 tables: **users**, **attendance**, **admin**.
2. Click **`users`** → then click **Structure** tab.  
   - Screenshot this so columns (id, name, email, roll_number, face_encoding, created_at) and types/keys are visible.
3. Do the same for **attendance** (id, user_id, date, time, status, created_at) and **admin** (id, username, password, created_at).

**Option A – One combined screenshot:**  
- Click **`attendance_system`** in the left sidebar so the right side shows all 3 tables.  
- Then open each table’s **Structure** in a new tab and arrange browser windows, **or**  
- Take one screenshot of the left panel (database + 3 tables) and one of the **Structure** of each table, then paste into one image if your report allows.

**Option B – Single “schema” screenshot:**  
- Stay on **attendance_system** and make sure the left sidebar shows:  
  `attendance_system` → users, attendance, admin.  
- Take one screenshot and then take separate screenshots of each table’s **Structure** tab (so column names, types, primary/foreign keys are visible).

Save the main screenshot as: **Database_Schema_phpMyAdmin_Users_Attendance_Admin.png**

---

## Summary – DB login (Docker)

| Item        | Value              |
|------------|--------------------|
| **URL**    | http://localhost:8080 |
| **Username** | `root` or `appuser` |
| **Password** | `rootpassword` or `apppassword` |
| **Database** | `attendance_system` |
| **Tables** | users, attendance, admin |

---

## If phpMyAdmin is not starting

Run from this folder:

```bash
docker compose up -d
```

This starts/restarts all services including phpMyAdmin. Then open http://localhost:8080 again.
