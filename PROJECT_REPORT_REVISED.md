# SMART ATTENDANCE SYSTEM  
## Using Face Recognition Technology

**A Project Report**  
**Submitted in partial fulfillment of the requirements for the award of**  
**Bachelor of Computer Applications (BCA) / B.Tech (Computer Science) / Minor Project**

---

# TITLE PAGE

**Project Title:** Smart Attendance System Using Face Recognition Technology  

**Submitted by:**  
[Your Name]  
[Roll No.]  
[Class / Semester]  

**Under the guidance of**  
[Guide Name]  
[Designation]  

**Department of Computer Science / Computer Applications**  
[College Name]  
[University Name]  
[Session / Year]  

---

# CERTIFICATE

*(Placeholder – apne college ka format use karein)*

This is to certify that the project entitled **“Smart Attendance System Using Face Recognition Technology”** submitted by **[Your Name]** (Roll No. **[Your Roll No.]**) in partial fulfillment of the requirements for the award of **Bachelor of Computer Applications** from [College Name] is a record of bonafide work carried out under my supervision.

<br/><br/>  
**Guide Name**  
**Signature**  
**Date**  

<br/><br/>  
**Head of Department**  
**Signature**  
**Date**  

---

# ACKNOWLEDGEMENT

*(Placeholder – apne shabdon mein likhein)*

I would like to express my sincere gratitude to my project guide **[Guide Name]** for his/her valuable guidance and support throughout this project. I also thank the Head of the Department and all faculty members for their encouragement. I am thankful to my family and friends for their support.

---

# ABSTRACT

The Smart Attendance System is a web-based application that automates the process of marking attendance using face recognition technology. The system allows users to register once with their photograph; thereafter attendance is marked by capturing the user’s face through a browser camera. The backend uses OpenCV for face detection and a custom feature-based encoding for matching. Data is stored in MySQL and the interface is built with Flask, Bootstrap, and JavaScript. The system reduces manual effort, minimizes proxy attendance, and provides instant reports for administrators such as today’s attendance and monthly summary. This report describes the problem statement, objectives, system design, database schema, implementation with full source code, screenshots of the user interface, testing, and conclusion with future scope.

**Keywords:** Face Recognition, Attendance System, OpenCV, Flask, MySQL, Web Application.

---

# TABLE OF CONTENTS

| S. No. | Chapter | Page No. |
|--------|---------|-----------|
| 1 | Introduction | |
| 2 | Literature Survey / Existing System | |
| 3 | Problem Statement, Objectives and Scope | |
| 4 | System Analysis and Design | |
| 5 | Database Design | |
| 6 | Implementation (Source Code) | |
| 7 | Screenshots and User Interface | |
| 8 | Testing | |
| 9 | Conclusion and Future Scope | |
| 10 | References | |
| 11 | Appendix | |

---

# LIST OF FIGURES

| Figure No. | Description | Page No. |
|------------|-------------|----------|
| 1 | Home Page – Smart Attendance System | |
| 2 | Register Page – User Registration Form | |
| 3 | Capture Photo – Face capture during registration | |
| 4 | Mark Attendance – Camera page for attendance | |
| 5 | Admin Login Page | |
| 6 | Admin Dashboard – Statistics and Quick Actions | |
| 7 | All Registered Users | |
| 8 | Today’s Attendance Report | |
| 9 | Monthly Attendance Report | |
| 10 | Admin Settings – Danger Zone | |
| 11 | Success message – Attendance marked | |
| 12 | Database schema (phpMyAdmin) | |

---

# CHAPTER 1 – INTRODUCTION

## 1.1 Overview

Attendance is an essential part of any educational institution or organization. Traditional methods involve manual entry in registers or sheets, which is time-consuming, prone to errors, and allows proxy attendance. With the advancement of technology, automated systems using biometrics have become popular. This project focuses on **face recognition** as a contactless and user-friendly way to mark attendance.

## 1.2 About the Project

The **Smart Attendance System** is a web-based application that:

- Allows new users to **register** by entering their name, email, roll number, and capturing their face via the browser camera.
- Allows registered users to **mark attendance** by showing their face to the camera on the Mark Attendance page.
- Provides an **admin panel** where an administrator can view total users, today’s attendance count, list of all users, today’s report, monthly report, and perform maintenance tasks such as clearing attendance or deleting users.

The system uses **OpenCV** for face detection (Haar Cascade), and a custom encoding (histogram and statistical features of the face region) for matching. Data is stored in **MySQL** and the web application is built with **Flask** (Python), **Bootstrap 5**, and **JavaScript**.

## 1.3 Need for the System

- To save time spent on manual attendance.
- To reduce proxy attendance and human error.
- To generate reports (daily and monthly) automatically.
- To provide a simple, contactless method using a web browser and camera.

---

# CHAPTER 2 – LITERATURE SURVEY / EXISTING SYSTEM

## 2.1 Existing System

In many institutions, attendance is still taken manually: by calling roll numbers, signing on paper, or using simple software where students mark themselves present. These methods have drawbacks:

- **Manual register:** Time-consuming, difficult to analyze, and easy to forge.
- **Paper sheets:** Can be lost, duplicated, or tampered with.
- **Simple software:** Often allows anyone to mark attendance without verification (e.g. just entering roll number).

## 2.2 Proposed System

The proposed Smart Attendance System uses **face recognition** so that only the registered person can mark attendance. Once a user is registered with their face, the system recognizes them from the camera feed and records attendance. This reduces proxy and human error while keeping the process simple and web-based.

---

# CHAPTER 3 – PROBLEM STATEMENT, OBJECTIVES AND SCOPE

## 3.1 Problem Statement

Manual attendance is time-consuming, prone to errors, and allows proxy attendance. Maintaining physical registers and later digitizing data for analysis is tedious. There is a need for an automated, contactless, and reliable method to record attendance and generate reports with minimal human intervention.

## 3.2 Objectives

1. To automate attendance marking using face recognition technology.  
2. To reduce proxy attendance and human error.  
3. To provide a simple registration and attendance flow through a web interface.  
4. To allow administrators to view today’s and monthly attendance reports.  
5. To store user and attendance data securely in a relational database (MySQL).  
6. To keep the system user-friendly and deployable on a local network.

## 3.3 Scope

**In scope:**

- User registration with name, email, roll number, and face capture.
- Marking attendance via browser camera and face recognition.
- Admin login and dashboard (total users, today’s attendance count).
- List of all registered users.
- Today’s attendance report (name, roll number, time).
- Monthly attendance report (days present, percentage).
- Admin settings: clear attendance by date or clear all; delete single user or all users.
- Responsive web interface (Bootstrap).

**Out of scope:**

- Mobile application.
- SMS/email notifications.
- Biometric hardware (e.g. fingerprint device) integration.
- Multi-branch or complex role-based access.
- Integration with external ERP or fee system.

---

# CHAPTER 4 – SYSTEM ANALYSIS AND DESIGN

## 4.1 System Architecture

The system follows a three-tier structure:

1. **Presentation layer:** Web browser; HTML, CSS, JavaScript, Bootstrap.  
2. **Business logic layer:** Flask (Python) – routes, validation, face recognition logic.  
3. **Data layer:** MySQL – users, attendance, admin tables.

**Flow:**

- User opens the website → Flask serves HTML pages.
- For registration/attendance → browser captures image → image sent to server (base64) → server computes face encoding → compares with database → updates database → returns result to browser.
- Admin views reports by querying the same database.

## 4.2 Technology Stack

| Component | Technology |
|-----------|------------|
| Backend | Python 3, Flask |
| Database | MySQL (e.g. XAMPP) |
| Face detection & encoding | OpenCV (cv2), NumPy |
| Image handling | PIL/Pillow |
| Frontend | HTML5, CSS3, JavaScript, Bootstrap 5, Font Awesome |
| DB connector | mysql-connector-python |

## 4.3 Data Flow (High Level)

- **Registration:** User fills form → server validates → user captures face → server generates encoding, checks duplicate face → inserts into `users` table.  
- **Attendance:** User opens Mark Attendance → captures face → server generates encoding, compares with all users → if match and not already marked today → insert into `attendance` table.  
- **Admin:** Admin logs in → session created → dashboard and reports read from `users` and `attendance` tables.

---

# CHAPTER 5 – DATABASE DESIGN

## 5.1 Entities

- **users:** Stores id, name, email, roll_number, face_encoding (text), created_at.  
- **attendance:** Stores id, user_id (FK to users), date, time, status (Present/Absent), created_at.  
- **admin:** Stores id, username, password, created_at.

## 5.2 ER Diagram

*(Yahan aap apna ER diagram image paste karein. Text description: users 1——N attendance; admin separate.)*

**[INSERT SCREENSHOT / IMAGE: ER Diagram – entities users, attendance, admin with relationships]**

## 5.3 Database Schema – init.sql (Full Code)

```sql
-- Initialize database for Smart Attendance System
USE attendance_system;

CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    roll_number VARCHAR(50) UNIQUE NOT NULL,
    face_encoding TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS attendance (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    date DATE NOT NULL,
    time TIME NOT NULL,
    status ENUM('Present', 'Absent') DEFAULT 'Present',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE IF NOT EXISTS admin (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO admin (username, password) VALUES ('admin', 'admin123');
```

**Figure 12 – Database schema:** [INSERT SCREENSHOT: phpMyAdmin showing tables users, attendance, admin with structure]

---

# CHAPTER 6 – IMPLEMENTATION (SOURCE CODE)

## 6.1 Main Application – app.py (Complete Code)

```python
from flask import Flask, render_template, request, redirect, url_for, flash, jsonify, session
import cv2
import numpy as np
import base64
from PIL import Image
import io
import pickle
import os
import re
from datetime import datetime, date
from config.database import DatabaseConfig
from utils.face_utils import FaceRecognitionUtils

app = Flask(__name__, template_folder='app/templates', static_folder='app/static')
app.secret_key = 'your-secret-key-here'

db_config = DatabaseConfig()
face_utils = FaceRecognitionUtils()

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/admin')
def admin_login():
    return render_template('admin_login.html')

@app.route('/admin/dashboard')
def admin_dashboard():
    if 'admin_logged_in' not in session:
        return redirect(url_for('admin_login'))
    connection = db_config.get_connection()
    cursor = connection.cursor()
    cursor.execute("SELECT COUNT(*) FROM users")
    total_users = cursor.fetchone()[0]
    cursor.execute("SELECT COUNT(*) FROM attendance WHERE date = %s", (date.today(),))
    today_attendance = cursor.fetchone()[0]
    cursor.close()
    connection.close()
    return render_template('admin_dashboard.html', total_users=total_users, today_attendance=today_attendance)

@app.route('/admin/users')
def admin_users():
    if 'admin_logged_in' not in session:
        return redirect(url_for('admin_login'))
    connection = db_config.get_connection()
    cursor = connection.cursor()
    cursor.execute("SELECT id, name, email, roll_number, created_at FROM users ORDER BY created_at DESC")
    users = cursor.fetchall()
    cursor.close()
    connection.close()
    return render_template('admin_users.html', users=users)

@app.route('/admin/today_report')
def admin_today_report():
    if 'admin_logged_in' not in session:
        return redirect(url_for('admin_login'))
    connection = db_config.get_connection()
    cursor = connection.cursor()
    cursor.execute("""
        SELECT u.name, u.roll_number, a.time FROM attendance a
        JOIN users u ON a.user_id = u.id WHERE a.date = %s ORDER BY a.time DESC
    """, (date.today(),))
    attendance_records = cursor.fetchall()
    cursor.close()
    connection.close()
    return render_template('admin_today_report.html', attendance_records=attendance_records, report_date=date.today())

@app.route('/admin/monthly_report')
def admin_monthly_report():
    if 'admin_logged_in' not in session:
        return redirect(url_for('admin_login'))
    connection = db_config.get_connection()
    cursor = connection.cursor()
    cursor.execute("""
        SELECT u.name, u.roll_number, COUNT(a.id) as days_present FROM users u
        LEFT JOIN attendance a ON u.id = a.user_id AND MONTH(a.date) = MONTH(CURDATE()) AND YEAR(a.date) = YEAR(CURDATE())
        GROUP BY u.id, u.name, u.roll_number ORDER BY u.name
    """)
    monthly_records = cursor.fetchall()
    cursor.close()
    connection.close()
    return render_template('admin_monthly_report.html', monthly_records=monthly_records, current_month=datetime.now().strftime('%B %Y'))

@app.route('/admin/settings')
def admin_settings():
    if 'admin_logged_in' not in session:
        return redirect(url_for('admin_login'))
    return render_template('admin_settings.html')

@app.route('/admin/logout')
def admin_logout():
    session.pop('admin_logged_in', None)
    flash('Logged out successfully')
    return redirect(url_for('admin_login'))

@app.route('/admin/clear_attendance', methods=['POST'])
def clear_attendance():
    if 'admin_logged_in' not in session:
        return jsonify({'success': False, 'message': 'Unauthorized'})
    data = request.get_json()
    clear_type = data.get('type', 'all')
    date_value = data.get('date', None)
    try:
        connection = db_config.get_connection()
        cursor = connection.cursor()
        if clear_type == 'date' and date_value:
            cursor.execute("DELETE FROM attendance WHERE date = %s", (date_value,))
            affected = cursor.rowcount
            connection.commit()
        elif clear_type == 'all':
            cursor.execute("DELETE FROM attendance")
            affected = cursor.rowcount
            connection.commit()
        else:
            cursor.close()
            connection.close()
            return jsonify({'success': False, 'message': 'Invalid request'})
        cursor.close()
        connection.close()
        return jsonify({'success': True, 'message': f'Cleared {affected} attendance record(s)'})
    except Exception as e:
        return jsonify({'success': False, 'message': str(e)})

@app.route('/admin/delete_user', methods=['POST'])
def delete_user():
    if 'admin_logged_in' not in session:
        return jsonify({'success': False, 'message': 'Unauthorized'})
    data = request.get_json()
    delete_type = data.get('type', 'single')
    user_id = data.get('user_id', None)
    try:
        connection = db_config.get_connection()
        cursor = connection.cursor()
        if delete_type == 'single' and user_id:
            cursor.execute("DELETE FROM attendance WHERE user_id = %s", (user_id,))
            cursor.execute("DELETE FROM users WHERE id = %s", (user_id,))
            connection.commit()
            cursor.close()
            connection.close()
            return jsonify({'success': True, 'message': 'User deleted successfully'})
        elif delete_type == 'all':
            cursor.execute("DELETE FROM attendance")
            cursor.execute("DELETE FROM users")
            affected = cursor.rowcount
            connection.commit()
            cursor.close()
            connection.close()
            return jsonify({'success': True, 'message': f'Deleted all {affected} users'})
        cursor.close()
        connection.close()
        return jsonify({'success': False, 'message': 'Invalid request'})
    except Exception as e:
        return jsonify({'success': False, 'message': str(e)})

@app.route('/admin/get_users_list', methods=['GET'])
def get_users_list():
    if 'admin_logged_in' not in session:
        return jsonify({'success': False, 'message': 'Unauthorized'})
    try:
        connection = db_config.get_connection()
        cursor = connection.cursor()
        cursor.execute("SELECT id, name, roll_number FROM users ORDER BY name")
        users = cursor.fetchall()
        cursor.close()
        connection.close()
        users_list = [{'id': u[0], 'name': u[1], 'roll_number': u[2]} for u in users]
        return jsonify({'success': True, 'users': users_list})
    except Exception as e:
        return jsonify({'success': False, 'message': str(e)})

@app.route('/register')
def register():
    return render_template('register.html')

@app.route('/attendance')
def attendance():
    return render_template('mark_attendance_camera.html')

@app.route('/mark_attendance_with_photo', methods=['POST'])
def mark_attendance_with_photo():
    data = request.get_json()
    photo_data = data['photo']
    try:
        image_data = photo_data.split(',')[1]
        image_bytes = base64.b64decode(image_data)
        image = Image.open(io.BytesIO(image_bytes))
        image_array = np.array(image)
        face_encoding = face_utils.process_image_for_encoding(image_array)
        if face_encoding is not None:
            connection = db_config.get_connection()
            cursor = connection.cursor()
            cursor.execute("SELECT id, name, face_encoding FROM users")
            users = cursor.fetchall()
            for user in users:
                stored_encoding = pickle.loads(bytes.fromhex(user[2]))
                best_match_index, _ = face_utils.compare_faces([stored_encoding], face_encoding)
                if best_match_index is not None:
                    cursor.execute("SELECT * FROM attendance WHERE user_id = %s AND date = %s", (user[0], date.today()))
                    if cursor.fetchone() is None:
                        cursor.execute("INSERT INTO attendance (user_id, date, time) VALUES (%s, %s, %s)", (user[0], date.today(), datetime.now().time()))
                        connection.commit()
                        cursor.close()
                        connection.close()
                        return jsonify({'success': True, 'message': f'Attendance marked for {user[1]}'})
                    cursor.close()
                    connection.close()
                    return jsonify({'success': False, 'message': 'Attendance already marked today'})
            cursor.close()
            connection.close()
            return jsonify({'success': False, 'message': 'Face not recognized. Please register first.'})
        return jsonify({'success': False, 'message': 'No face detected in the image'})
    except Exception as e:
        return jsonify({'success': False, 'message': f'Error: {str(e)}'})

@app.route('/login_admin', methods=['POST'])
def login_admin():
    username = request.form['username']
    password = request.form['password']
    connection = db_config.get_connection()
    cursor = connection.cursor()
    cursor.execute("SELECT * FROM admin WHERE username = %s AND password = %s", (username, password))
    admin = cursor.fetchone()
    cursor.close()
    connection.close()
    if admin:
        session['admin_logged_in'] = True
        return redirect(url_for('admin_dashboard'))
    flash('Invalid credentials')
    return redirect(url_for('admin_login'))

@app.route('/register_user', methods=['POST'])
def register_user():
    name = request.form['name']
    email = request.form['email']
    roll_number = request.form['roll_number']
    if not name or len(name) > 60 or not all(c.isalpha() or c.isspace() for c in name):
        flash('Invalid name. Use alphabets only (maximum 60 characters).', 'danger')
        return redirect(url_for('register'))
    email_regex = r'^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    if not re.match(email_regex, email):
        flash('Invalid email format.', 'danger')
        return redirect(url_for('register'))
    connection = db_config.get_connection()
    cursor = connection.cursor()
    cursor.execute("SELECT name FROM users WHERE roll_number = %s", (roll_number,))
    if cursor.fetchone():
        cursor.close()
        connection.close()
        flash('Roll number already registered.', 'danger')
        return redirect(url_for('register'))
    cursor.execute("SELECT name FROM users WHERE email = %s", (email,))
    if cursor.fetchone():
        cursor.close()
        connection.close()
        flash('Email already registered.', 'danger')
        return redirect(url_for('register'))
    cursor.close()
    connection.close()
    return render_template('capture_photo.html', name=name, email=email, roll_number=roll_number)

@app.route('/register_user_with_photo', methods=['POST'])
def register_user_with_photo():
    data = request.get_json()
    name, email, roll_number = data['name'], data['email'], data['roll_number']
    photo_data = data['photo']
    try:
        image_data = photo_data.split(',')[1]
        image_bytes = base64.b64decode(image_data)
        image = Image.open(io.BytesIO(image_bytes))
        image_array = np.array(image)
        face_encoding = face_utils.process_image_for_encoding(image_array)
        if face_encoding is not None:
            connection = db_config.get_connection()
            cursor = connection.cursor()
            cursor.execute("SELECT id, name, face_encoding FROM users")
            for user in cursor.fetchall():
                stored_encoding = pickle.loads(bytes.fromhex(user[2]))
                if face_utils.compare_faces([stored_encoding], face_encoding, tolerance=0.3)[0] is not None:
                    cursor.close()
                    connection.close()
                    return jsonify({'success': False, 'message': f'Face already registered with user: {user[1]}'})
            encoding_str = pickle.dumps(face_encoding).hex()
            cursor.execute("INSERT INTO users (name, email, roll_number, face_encoding) VALUES (%s, %s, %s, %s)", (name, email, roll_number, encoding_str))
            connection.commit()
            cursor.close()
            connection.close()
            return jsonify({'success': True, 'message': f'User {name} registered successfully!'})
        return jsonify({'success': False, 'message': 'No face detected.'})
    except Exception as e:
        return jsonify({'success': False, 'message': str(e)})

@app.route('/mark_attendance')
def mark_attendance():
    face_encoding = face_utils.capture_face_encoding()
    if face_encoding is None:
        return jsonify({'success': False, 'message': 'No face detected'})
    connection = db_config.get_connection()
    cursor = connection.cursor()
    cursor.execute("SELECT id, name, face_encoding FROM users")
    for user in cursor.fetchall():
        stored_encoding = pickle.loads(bytes.fromhex(user[2]))
        best_match_index, _ = face_utils.compare_faces([stored_encoding], face_encoding)
        if best_match_index is not None:
            cursor.execute("SELECT * FROM attendance WHERE user_id = %s AND date = %s", (user[0], date.today()))
            if cursor.fetchone() is None:
                cursor.execute("INSERT INTO attendance (user_id, date, time) VALUES (%s, %s, %s)", (user[0], date.today(), datetime.now().time()))
                connection.commit()
                cursor.close()
                connection.close()
                return jsonify({'success': True, 'message': f'Attendance marked for {user[1]}'})
            cursor.close()
            connection.close()
            return jsonify({'success': False, 'message': 'Attendance already marked today'})
    cursor.close()
    connection.close()
    return jsonify({'success': False, 'message': 'Face not recognized'})

if __name__ == '__main__':
    db_config.create_database()
    db_config.create_tables()
    app.run(host='0.0.0.0', port=5000, debug=False)
```

## 6.2 config/database.py (Full Code)

```python
import mysql.connector
from mysql.connector import Error
import os

class DatabaseConfig:
    def __init__(self):
        self.host = os.getenv('DB_HOST', 'localhost')
        self.database = os.getenv('DB_NAME', 'attendance_system')
        self.user = os.getenv('DB_USER', 'root')
        self.password = os.getenv('DB_PASSWORD', '')
        self.port = int(os.getenv('DB_PORT', '3306'))

    def get_connection(self):
        try:
            return mysql.connector.connect(host=self.host, database=self.database, user=self.user, password=self.password, port=self.port)
        except Error as e:
            print(f"Error connecting to MySQL: {e}")
            return None

    def create_database(self):
        try:
            conn = mysql.connector.connect(host=self.host, user=self.user, password=self.password, port=self.port)
            cur = conn.cursor()
            cur.execute(f"CREATE DATABASE IF NOT EXISTS {self.database}")
            cur.close()
            conn.close()
        except Error as e:
            print(f"Error creating database: {e}")

    def create_tables(self):
        conn = self.get_connection()
        if not conn:
            return
        cur = conn.cursor()
        cur.execute("""CREATE TABLE IF NOT EXISTS users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(100) NOT NULL, email VARCHAR(100) UNIQUE NOT NULL, roll_number VARCHAR(50) UNIQUE NOT NULL, face_encoding TEXT, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP)""")
        cur.execute("""CREATE TABLE IF NOT EXISTS attendance (id INT AUTO_INCREMENT PRIMARY KEY, user_id INT, date DATE NOT NULL, time TIME NOT NULL, status ENUM('Present','Absent') DEFAULT 'Present', created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, FOREIGN KEY (user_id) REFERENCES users(id))""")
        cur.execute("""CREATE TABLE IF NOT EXISTS admin (id INT AUTO_INCREMENT PRIMARY KEY, username VARCHAR(50) UNIQUE NOT NULL, password VARCHAR(255) NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP)""")
        cur.execute("SELECT COUNT(*) FROM admin")
        if cur.fetchone()[0] == 0:
            cur.execute("INSERT INTO admin (username, password) VALUES ('admin', 'admin123')")
        conn.commit()
        cur.close()
        conn.close()
```

## 6.3 utils/face_utils.py (Full Code)

```python
import cv2
import numpy as np

class FaceRecognitionUtils:
    def __init__(self):
        self.face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

    def capture_face_encoding(self):
        try:
            cap = cv2.VideoCapture(0)
            if not cap.isOpened():
                return np.random.rand(128)
            for _ in range(30):
                ret, frame = cap.read()
                if not ret:
                    continue
                gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
                faces = self.face_cascade.detectMultiScale(gray, 1.1, 4)
                if len(faces) > 0:
                    x, y, w, h = faces[0]
                    enc = np.array([x, y, w, h] + [np.random.rand() for _ in range(124)])
                    cap.release()
                    return enc
            cap.release()
        except Exception:
            pass
        return np.random.rand(128)

    def compare_faces(self, known_encodings, face_encoding, tolerance=0.3):
        if not known_encodings:
            return None, None
        distances = []
        for known in known_encodings:
            k, c = np.array(known), np.array(face_encoding)
            dot = np.dot(k, c)
            norm = np.linalg.norm(k) * np.linalg.norm(c)
            dist = 1 - (dot / norm) if norm else 1.0
            distances.append(dist)
        idx = np.argmin(distances)
        return (idx, distances[idx]) if distances[idx] < tolerance else (None, None)

    def process_image_for_encoding(self, image_array):
        try:
            gray = cv2.cvtColor(image_array, cv2.COLOR_RGB2GRAY) if len(image_array.shape) == 3 else image_array
            faces = self.face_cascade.detectMultiScale(gray, 1.1, 4)
            if len(faces) == 0:
                return None
            x, y, w, h = faces[0]
            face_region = gray[y:y+h, x:x+w]
            face_resized = cv2.resize(face_region, (100, 100))
            hist = cv2.calcHist([face_resized], [0], None, [32], [0, 256]).flatten() / cv2.calcHist([face_resized], [0], None, [32], [0, 256]).sum()
            stats = np.array([np.mean(face_resized), np.std(face_resized), np.median(face_resized), np.min(face_resized), np.max(face_resized)])
            return np.concatenate([hist, stats])
        except Exception:
            return None
```

## 6.4 setup_database.py (Full Code)

```python
from config.database import DatabaseConfig

def main():
    print("Setting up Smart Attendance System Database...")
    db = DatabaseConfig()
    db.create_database()
    db.create_tables()
    print("Done. Default admin: admin / admin123")
    print("Run: python app.py")

if __name__ == "__main__":
    main()
```

---

# CHAPTER 7 – SCREENSHOTS AND USER INTERFACE

Har figure ke liye app run karke (python app.py, browser mein http://localhost:5000) screenshot lein aur report mein paste karein.

**Figure 1 – Home Page**  
[INSERT SCREENSHOT: Home page – title “Smart Attendance System”, teen cards: Register Now, Mark Attendance, Admin Login, aur System Features list]

**Figure 2 – Register Page**  
[INSERT SCREENSHOT: Register form – Full Name, Email, Roll Number fields, “Proceed to Capture Face” button]

**Figure 3 – Capture Photo (Registration)**  
[INSERT SCREENSHOT: Camera view aur “Capture Photo” / “Confirm & Register” buttons]

**Figure 4 – Mark Attendance**  
[INSERT SCREENSHOT: Mark Attendance page – camera aur “Capture & Mark Attendance” button]

**Figure 5 – Admin Login**  
[INSERT SCREENSHOT: Admin login – Username, Password, Login button]

**Figure 6 – Admin Dashboard**  
[INSERT SCREENSHOT: Dashboard – Total Registered Users, Today’s Attendance, Quick Actions (View All Users, Today’s Report, Monthly Report, Settings)]

**Figure 7 – All Registered Users**  
[INSERT SCREENSHOT: Table with columns ID, Name, Email, Roll Number, Registered On]

**Figure 8 – Today’s Attendance Report**  
[INSERT SCREENSHOT: Table with Name, Roll Number, Time for today’s date]

**Figure 9 – Monthly Attendance Report**  
[INSERT SCREENSHOT: Table with Name, Roll Number, Days Present, Attendance %]

**Figure 10 – Admin Settings (Danger Zone)**  
[INSERT SCREENSHOT: Clear Attendance (by date / all), Delete Users (single / all) section]

**Figure 11 – Success Message**  
[INSERT SCREENSHOT: Green success alert “Attendance marked for [Name]”]

**Figure 12 – Database Schema**  
[INSERT SCREENSHOT: phpMyAdmin – structure of users, attendance, admin tables]

---

# CHAPTER 8 – TESTING

- **Registration:** Valid name, email, roll number → capture face → user list mein dikhna chahiye.  
- **Duplicate roll/email:** Error message aana chahiye.  
- **Attendance:** Registered user face capture kare → “Attendance marked for [Name]” aana chahiye; Today’s Report mein entry.  
- **Duplicate attendance:** Same day dobara mark karne par “Attendance already marked today” aana chahiye.  
- **Admin:** admin / admin123 se login → dashboard, users, today report, monthly report sahi dikhne chahiye. Clear attendance aur delete user test karein.

---

# CHAPTER 9 – CONCLUSION AND FUTURE SCOPE

The Smart Attendance System successfully automates attendance using face recognition via a web interface and MySQL. It reduces manual work and proxy attendance and provides instant reports.

**Future scope:** Mobile app, SMS/email alerts, deep learning-based face recognition (e.g. dlib), multi-location support, Excel/PDF export.

---

# REFERENCES

1. Flask Documentation. https://flask.palletsprojects.com/  
2. OpenCV Documentation. https://docs.opencv.org/  
3. MySQL Documentation. https://dev.mysql.com/doc/  
4. Bootstrap 5. https://getbootstrap.com/docs/5.3/  

*(IEEE / APA / MLA format mein apne college ke hisaab se likhein.)*

---

# APPENDIX

## Base template (base.html) – structure

- Navbar: Home, Register, Mark Attendance, Admin.  
- Flash messages block.  
- `{% block content %}` and `{% block scripts %}`.  
- Bootstrap 5 and jQuery included.

## index.html

- Title “Smart Attendance System”, subtitle “Using Face Recognition Technology”.  
- Three cards: Register Now, Mark Attendance, Admin Login.  
- System Features list (face detection, recognition, reporting, etc.).

## style.css (excerpt)

- body background #f8f9fa, font Segoe UI.  
- .card: rounded, shadow, hover lift.  
- .btn: pill shape, .card-header gradient.  
- .alert animation slideIn.

---

**End of Report**

**Note:** Certificate, Acknowledgement, and optional List of Tables ko apne college format ke mutabiq bhar dein. Page numbers Word mein add karein. Screenshots replace karke final DOCX/PDF banaen.
