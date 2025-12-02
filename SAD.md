# VPES-SIMS System Architecture Diagram

> **Detailed System Architecture for Vinzons Pilot Elementary School - Student Information Management System**
> 
> Generated: December 2, 2025

---

## 📐 Complete System Architecture

```mermaid
graph TB
    subgraph LAYER1["🎨 LAYER 1: PRESENTATION LAYER"]
        direction LR
        P1["<b>Student Dashboard</b><br/>View Grades<br/>Check Application Status"]
        P2["<b>Teacher Dashboard</b><br/>Manage Classes<br/>Enter Grades"]
        P3["<b>Admin Dashboard</b><br/>System Management<br/>User Control"]
    end

    subgraph LAYER2["🔐 LAYER 2: SECURITY LAYER"]
        direction LR
        S1["<b>Firebase Auth</b><br/>OAuth 2.0<br/>JWT Tokens"]
        S2["<b>Role-Based Access</b><br/>Admin/Teacher/Student<br/>Permission Control"]
        S3["<b>Session Management</b><br/>Token Validation<br/>Secure Sessions"]
    end

    subgraph LAYER3["⚙️ LAYER 3: APPLICATION LAYER - Backend"]
        direction TB
        
        subgraph APP_COL1["Core APIs"]
            A1["<b>Login System</b><br/>api/login.php<br/>User Authentication"]
            A2["<b>Application Management</b><br/>api/submit_application.php<br/>api/get_application.php"]
        end
        
        subgraph APP_COL2["Learning Tools"]
            A3["<b>Enrollment System</b><br/>Application Processing<br/>Student Registration"]
            A4["<b>Section Assignment</b><br/>api/students/move.php<br/>Class Management"]
            A5["<b>Grade Management</b><br/>api/teachers/*<br/>Score Entry"]
            A6["<b>Document Verification</b><br/>api/verify_document.php<br/>File Management"]
        end
        
        subgraph APP_COL3["Support Systems"]
            A7["<b>Email Queue System</b><br/>Notifications<br/>SMTP Delivery"]
            A8["<b>Settings Manager</b><br/>api/settings/public.php<br/>System Config"]
        end
    end

    subgraph LAYER4["💾 LAYER 4: DATA STORAGE & BUSINESS LOGIC"]
        direction TB
        
        subgraph DATA_COL1["Public Cloud Services"]
            D1["<b>Firebase Platform</b><br/>User Authentication<br/>Token Management"]
            D2["<b>PSGC API</b><br/>Location Services<br/>Address Validation"]
            D3["<b>Gmail SMTP</b><br/>Email Delivery<br/>Notifications"]
        end
        
        subgraph DATA_COL2["Private Cloud - Database"]
            D4["<b>MySQL Database</b><br/>vpes_sims"]
            D5["<b>Core Tables</b><br/>users, students<br/>applications, sections"]
            D6["<b>Supporting Tables</b><br/>grades, documents<br/>enrollments, logs"]
        end
        
        subgraph DATA_COL3["Private Cloud - File Storage"]
            D7["<b>Document Storage</b><br/>uploads/documents/<br/>PDF, Images"]
            D8["<b>Cache Files</b><br/>cache/<br/>JSON Data"]
            D9["<b>System Logs</b><br/>logs/<br/>Error Tracking"]
        end
    end

    subgraph LAYER5["🏗️ LAYER 5: INFRASTRUCTURE - Fixed Platform"]
        direction LR
        I1["<b>Web Server</b><br/>Apache XAMPP<br/>PHP 7.4+"]
        I2["<b>Database Server</b><br/>MySQL 8.0<br/>Local Storage"]
        I3["<b>Task Scheduler</b><br/>Windows Scheduler<br/>Cron Jobs"]
    end

    subgraph BUSINESS_LOGIC["Business Logic Classes"]
        direction LR
        BL1["<b>Application.php</b><br/>Enrollment Processing<br/>Location Decoding"]
        BL2["<b>User.php</b><br/>Firebase Sync<br/>User Management"]
        BL3["<b>Document.php</b><br/>File Upload<br/>Verification"]
        BL4["<b>Logger.php</b><br/>Activity Logs<br/>Audit Trail"]
    end

    subgraph CONFIG["Configuration Layer"]
        direction LR
        CF1["<b>database.php</b><br/>PDO Connection"]
        CF2["<b>firebase.php</b><br/>JWT Verification"]
        CF3["<b>email.php</b><br/>SMTP Config"]
        CF4["<b>Mailer.php</b><br/>Email Service"]
    end

    %% LAYER 1 to LAYER 2 Connections
    P1 -->|"Login Request"| S1
    P2 -->|"Auth Token"| S2
    P3 -->|"Admin Access"| S3

    %% LAYER 2 to LAYER 3 Connections
    S1 -->|"Verified Token"| A1
    S2 -->|"Role Check"| A2
    S3 -->|"Session Valid"| A3

    %% LAYER 3 Internal Connections
    A1 -.->|"User Data"| A2
    A2 -.->|"Enrollment"| A3
    A3 -.->|"Assign Section"| A4
    A4 -.->|"Student List"| A5
    A5 -.->|"Upload Docs"| A6
    A6 -.->|"Notify"| A7
    A7 -.->|"Config"| A8

    %% LAYER 3 to Business Logic
    A1 -->|"Authenticate"| BL2
    A2 -->|"Process App"| BL1
    A3 -->|"Create Record"| BL1
    A4 -->|"Update Student"| BL2
    A6 -->|"Verify File"| BL3
    A7 -->|"Log Event"| BL4

    %% Business Logic to Config
    BL1 -->|"Get Connection"| CF1
    BL1 -->|"Decode Location"| CF2
    BL2 -->|"DB Access"| CF1
    BL3 -->|"Store File"| CF1
    BL4 -->|"Write Log"| CF1
    A7 -->|"Send Email"| CF4

    %% Config to LAYER 4
    CF1 -->|"PDO Query"| D4
    CF2 -->|"API Call"| D1
    CF4 -->|"SMTP Send"| D3
    BL1 -->|"HTTP GET"| D2

    %% LAYER 4 Internal Database
    D4 -.->|"Schema"| D5
    D5 -.->|"Relations"| D6
    BL3 -->|"Save File"| D7
    BL1 -->|"Cache Data"| D8
    BL4 -->|"Write Logs"| D9

    %% LAYER 4 to LAYER 5
    D4 -->|"Storage"| I2
    D7 -->|"File System"| I2
    A7 -->|"Schedule"| I3
    
    %% LAYER 5 Infrastructure Support
    I1 -.->|"Hosts"| A1
    I1 -.->|"Runs PHP"| BL1
    I2 -.->|"Stores Data"| D4
    I3 -.->|"Executes Cron"| A7

    %% External Service Connections
    D1 -->|"Auth Response"| S1
    D2 -->|"Location Data"| BL1
    D3 -->|"Email Sent"| A7

    %% Styling - PRESENTATION LAYER (Red)
    style P1 fill:#e57373,stroke:#c62828,stroke-width:3px,color:#fff
    style P2 fill:#e57373,stroke:#c62828,stroke-width:3px,color:#fff
    style P3 fill:#e57373,stroke:#c62828,stroke-width:3px,color:#fff
    style LAYER1 fill:#ffebee,stroke:#c62828,stroke-width:4px

    %% Styling - SECURITY LAYER (Orange)
    style S1 fill:#ffb74d,stroke:#e65100,stroke-width:3px,color:#fff
    style S2 fill:#ffb74d,stroke:#e65100,stroke-width:3px,color:#fff
    style S3 fill:#ffb74d,stroke:#e65100,stroke-width:3px,color:#fff
    style LAYER2 fill:#fff3e0,stroke:#e65100,stroke-width:4px

    %% Styling - APPLICATION LAYER (Blue)
    style A1 fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#fff
    style A2 fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#fff
    style A3 fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#fff
    style A4 fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#fff
    style A5 fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#fff
    style A6 fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#fff
    style A7 fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#fff
    style A8 fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#fff
    style LAYER3 fill:#e3f2fd,stroke:#1565c0,stroke-width:4px
    style APP_COL1 fill:#bbdefb,stroke:#1976d2,stroke-width:2px
    style APP_COL2 fill:#bbdefb,stroke:#1976d2,stroke-width:2px
    style APP_COL3 fill:#bbdefb,stroke:#1976d2,stroke-width:2px

    %% Styling - DATA STORAGE LAYER (Green/Teal)
    style D1 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style D2 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style D3 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style D4 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style D5 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style D6 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style D7 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style D8 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style D9 fill:#4db6ac,stroke:#00695c,stroke-width:2px,color:#fff
    style LAYER4 fill:#e0f2f1,stroke:#00695c,stroke-width:4px
    style DATA_COL1 fill:#b2dfdb,stroke:#00796b,stroke-width:2px
    style DATA_COL2 fill:#b2dfdb,stroke:#00796b,stroke-width:2px
    style DATA_COL3 fill:#b2dfdb,stroke:#00796b,stroke-width:2px

    %% Styling - INFRASTRUCTURE LAYER (Purple)
    style I1 fill:#9575cd,stroke:#4527a0,stroke-width:3px,color:#fff
    style I2 fill:#9575cd,stroke:#4527a0,stroke-width:3px,color:#fff
    style I3 fill:#9575cd,stroke:#4527a0,stroke-width:3px,color:#fff
    style LAYER5 fill:#ede7f6,stroke:#4527a0,stroke-width:4px

    %% Styling - Business Logic (Yellow)
    style BL1 fill:#fff59d,stroke:#f57f17,stroke-width:2px
    style BL2 fill:#fff59d,stroke:#f57f17,stroke-width:2px
    style BL3 fill:#fff59d,stroke:#f57f17,stroke-width:2px
    style BL4 fill:#fff59d,stroke:#f57f17,stroke-width:2px
    style BUSINESS_LOGIC fill:#fffde7,stroke:#f57f17,stroke-width:3px

    %% Styling - Configuration (Light Green)
    style CF1 fill:#aed581,stroke:#558b2f,stroke-width:2px
    style CF2 fill:#aed581,stroke:#558b2f,stroke-width:2px
    style CF3 fill:#aed581,stroke:#558b2f,stroke-width:2px
    style CF4 fill:#aed581,stroke:#558b2f,stroke-width:2px
    style CONFIG fill:#f1f8e9,stroke:#558b2f,stroke-width:3px
```

---

## 🔄 Data Flow Descriptions

### 1. **User Authentication Flow**
```
User → Login Page → Firebase Auth SDK → Firebase Cloud → ID Token → API Login → 
Firebase Config (Verify Token) → User Class → Database → Session Created
```

### 2. **Application Submission Flow**
```
Student → Application Form → Applications Handler → API Submit Application → 
Application Class → PSGC API (Location Decode) → Database Tables (applications, 
application_personal_info, application_addresses, application_guardians) → 
Application Logs → Response to Student
```

### 3. **Document Upload Flow**
```
User → Documents Page → Documents Handler → API Upload Document → 
Document Class → File Storage (uploads/documents/) → Database (documents table) → 
Logger → Success Response
```

### 4. **Email Notification Flow**
```
System Event → Email Queue Table → Windows Task Scheduler → Process Queue Cron → 
Email Queue API → Mailer Service → SMTP Server → Email Sent → 
Update Queue Status
```

### 5. **Section Assignment Flow**
```
Admin → Sections Manager → API Section List → Database (sections, students) → 
Display Sections → Admin Assigns Student → API Students Move → 
Update student.section_id → Enrollment History Log
```

### 6. **Grade Entry Flow**
```
Teacher → Teacher Dashboard → Teacher APIs → Get Assignments → Database → 
Display Students → Submit Grades → API Update Grades → Grades Table → 
Email Notification Queue
```

---

## 🏗️ Layer Descriptions

### **Client Layer (Frontend)**
- **Purpose**: User interface for different user roles
- **Technologies**: HTML5, CSS3, Bootstrap 5
- **Files**: `*.html` files in root directory
- **Access Control**: Role-based page access (Student, Admin, Teacher)

### **JavaScript Layer**
- **Purpose**: Client-side logic, API communication, authentication
- **Technologies**: ES6+ JavaScript, Firebase SDK, Fetch API
- **Files**: `JS/*.js`
- **Key Features**: Token management, API requests, dynamic UI updates

### **External Services Layer**
- **Firebase Authentication**: User authentication via Google OAuth 2.0
- **PSGC API**: Philippine Standard Geographic Code location lookup
- **Gmail SMTP**: Email delivery service (Port 587 TLS)
- **Google reCAPTCHA**: Bot protection on forms

### **API Gateway Layer**
- **Purpose**: RESTful API endpoints for all operations
- **Technologies**: PHP 7.4+, JSON responses
- **Files**: `api/**/*.php`
- **Security**: Bearer token authentication, CORS enabled

### **Configuration Layer**
- **Purpose**: System configuration and service initialization
- **Files**: `config/*.php`
- **Components**:
  - Database connection (PDO)
  - Firebase token verification (JWT)
  - Email SMTP settings
  - Mailer wrapper (PHPMailer)

### **Business Logic Layer**
- **Purpose**: Core application logic and business rules
- **Technologies**: Object-Oriented PHP
- **Files**: `classes/*.php`
- **Classes**:
  - `Application`: Handles enrollment applications, location decoding
  - `User`: Firebase user synchronization
  - `Document`: File upload and verification
  - `Logger`: Activity logging

### **Data Access Layer**
- **Purpose**: Database connection management
- **Technology**: PDO (PHP Data Objects)
- **Database**: MySQL 8.0+
- **Features**: Prepared statements, transaction support

### **Database Schema**
- **Core Tables**: 19 normalized tables
- **Key Entities**:
  - Users & Authentication
  - Applications (with normalized personal info, addresses, guardians)
  - Students (with addresses and guardians)
  - Sections & Enrollments
  - Teachers & Assignments
  - Documents & Grades
  - Logs & Settings

### **File Storage Layer**
- **Document Storage**: `uploads/documents/` (PDF, images)
- **Cache Storage**: JSON files for API responses
- **Logs**: Error and application logs

### **Background Jobs Layer**
- **Email Queue**: Asynchronous email sending
- **Automated Scheduler**: Class schedule generation
- **Cron Jobs**: Windows Task Scheduler integration

---

## 🔐 Security Architecture

### Authentication & Authorization
```mermaid
graph LR
    A[User Login] -->|Email/Password| B[Firebase Auth]
    B -->|ID Token| C[Backend API]
    C -->|Verify Token| D[Firebase Public Keys]
    D -->|Valid| E[Check User Role]
    E -->|Admin| F[Full Access]
    E -->|Teacher| G[Limited Access]
    E -->|Student| H[Own Data Only]
    
    style B fill:#4285f4,color:#fff
    style D fill:#4285f4,color:#fff
```

### Data Flow Security
- **Frontend → Backend**: HTTPS, Bearer Token in Authorization header
- **Backend → Database**: PDO prepared statements (SQL injection prevention)
- **Backend → External APIs**: HTTPS, timeout limits
- **File Uploads**: MIME type validation, file size limits
- **Token Caching**: Secure cache directory with 1-hour expiry

---

## 📊 Database Entity Relationship Overview

```mermaid
erDiagram
    USERS ||--o{ APPLICATIONS : creates
    USERS ||--o{ EMAIL_QUEUE : receives
    APPLICATIONS ||--|| APPLICATION_PERSONAL_INFO : has
    APPLICATIONS ||--o{ APPLICATION_ADDRESSES : has
    APPLICATIONS ||--o{ APPLICATION_GUARDIANS : has
    APPLICATIONS ||--o{ DOCUMENTS : submits
    APPLICATIONS ||--o{ APPLICATION_LOGS : generates
    APPLICATIONS ||--o| STUDENTS : "enrolls as"
    STUDENTS ||--o{ STUDENT_ADDRESSES : has
    STUDENTS ||--o{ STUDENT_GUARDIANS : has
    STUDENTS ||--o| SECTIONS : "assigned to"
    STUDENTS ||--o{ ENROLLMENTS : has
    STUDENTS ||--o{ GRADES : receives
    STUDENTS ||--o{ ENROLLMENT_HISTORY : tracks
    SECTIONS ||--o{ CLASS_SCHEDULES : contains
    SECTIONS ||--o{ TEACHER_SUBJECT_ASSIGNMENTS : taught_by
    TEACHERS ||--o{ TEACHER_SUBJECT_ASSIGNMENTS : teaches
    SYSTEM_SETTINGS ||--|| USERS : configures
```

---

## 🔧 Technology Stack Summary

| Layer | Technology | Version |
|-------|-----------|---------|
| **Frontend** | HTML5, CSS3, Bootstrap | 5.3.0 |
| **Client Script** | JavaScript (ES6+) | - |
| **Backend** | PHP | 7.4+ |
| **Database** | MySQL | 8.0+ |
| **Web Server** | Apache (XAMPP) | 3.3.0 |
| **Authentication** | Firebase Auth | SDK 10.x |
| **Email** | PHPMailer, Gmail SMTP | - |
| **External API** | PSGC GitLab API | v1 |
| **Caching** | File-based JSON cache | - |
| **Task Scheduler** | Windows Task Scheduler | - |

---

## 📝 API Endpoints Summary

| Category | Endpoint | Method | Purpose |
|----------|----------|--------|---------|
| **Auth** | `/api/login.php` | POST | User authentication |
| **Auth** | `/api/sync_user.php` | POST | Sync Firebase user to DB |
| **Applications** | `/api/submit_application.php` | POST | Submit enrollment |
| **Applications** | `/api/get_application.php` | GET | Get user application |
| **Applications** | `/api/get_all_applications.php` | GET | List all (admin) |
| **Documents** | `/api/upload_document_enhanced.php` | POST | Upload document |
| **Documents** | `/api/verify_document.php` | PUT | Verify/reject document |
| **Students** | `/api/students/get_all.php` | GET | List students |
| **Students** | `/api/students/move.php` | POST | Move to section |
| **Sections** | `/api/sections/list.php` | GET | List sections |
| **Teachers** | `/api/teachers/get_assignments.php` | GET | Get teacher schedule |
| **Admin** | `/api/admin_dashboard.php` | GET | Dashboard stats |
| **Settings** | `/api/settings/public.php` | GET | Public settings |

---

## 🎯 Key System Features

### ✅ Implemented Features
1. **Multi-role Authentication** (Student, Teacher, Admin)
2. **Online Enrollment Application** with normalized data structure
3. **Document Upload & Verification** with cloud storage
4. **Section Management** with student assignment
5. **Grade Entry System** for teachers
6. **Email Notification Queue** with background processing
7. **Location Decoding** with PSGC API integration and caching
8. **User Management** with Firebase sync
9. **Dashboard Analytics** for admin
10. **Responsive UI** with Bootstrap 5

### 🔄 Data Processing Patterns
- **Caching Strategy**: File-based cache for API responses (1-hour TTL)
- **Transaction Management**: Database transactions for multi-table operations
- **Error Handling**: Try-catch blocks with error logging
- **Normalization**: Separated tables for addresses, guardians, personal info
- **Audit Trail**: Application logs and enrollment history

---

## 📈 System Scalability Considerations

### Current Architecture
- **Monolithic PHP application** with modular class structure
- **File-based caching** (sufficient for school-level deployment)
- **Single MySQL database** (local XAMPP server)

### Future Enhancement Paths
1. **Redis/Memcached** for distributed caching
2. **CDN integration** for document delivery
3. **Load balancer** for multi-server deployment
4. **Microservices** separation (Auth, Enrollment, Grading)
5. **Message queue** (RabbitMQ) for async tasks
6. **Docker containerization** for easy deployment

---

**System Version**: Production Ready  
**Last Updated**: December 2, 2025  
**Architecture Type**: Layered MVC with Service-Oriented Components  
**Deployment**: XAMPP (Local/School Server)
