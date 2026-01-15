# Navigate to Downloads
cd ~/Downloads

# Extract
unzip jobportal-backend.zip

# Move to your workspace
mv jobportal-backend ~/Desktop/SpringBootProjects/
cd ~/Desktop/SpringBootProjects/jobportal-backend

# Open in VS Code
code .
```

---

## 🗂️ **STEP 3: UNDERSTAND PROJECT STRUCTURE (File Structure from Zero)**

When VS Code opens, you'll see this structure. Let me explain **EVERY SINGLE FOLDER/FILE**:
```
jobportal-backend/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── jobportal/
│   │   │           └── backend/
│   │   │               ├── JobportalBackendApplication.java  ← MAIN FILE (Starts app)
│   │   │               │
│   │   │               ├── controller/     ← [WE'LL CREATE] API endpoints (like restaurant waiters)
│   │   │               ├── service/        ← [WE'LL CREATE] Business logic (like chefs)
│   │   │               ├── repository/     ← [WE'LL CREATE] Database queries (like warehouse manager)
│   │   │               ├── model/          ← [WE'LL CREATE] Database tables (like menu items)
│   │   │               ├── dto/            ← [WE'LL CREATE] Request/Response formats (like order slips)
│   │   │               ├── config/         ← [WE'LL CREATE] Settings (like restaurant rules)
│   │   │               ├── exception/      ← [WE'LL CREATE] Error handling (like complaint manager)
│   │   │               └── util/           ← [WE'LL CREATE] Helper functions (like calculator)
│   │   │
│   │   └── resources/
│   │       ├── application.properties  ← CONFIGURATION FILE (database password, etc.)
│   │       ├── static/                 ← For images/CSS (we won't use - frontend separate)
│   │       └── templates/              ← For HTML pages (we won't use)
│   │
│   └── test/  ← Unit tests (Day 3)
│       └── java/...
│
├── target/  ← [AUTO-GENERATED] Compiled code (ignore this)
│
├── pom.xml  ← SHOPPING LIST! All dependencies listed here
├── mvnw     ← Maven wrapper (Linux/Mac)
├── mvnw.cmd ← Maven wrapper (Windows)
└── .gitignore
```

---

## 📚 **ANALOGY: Restaurant Management System**

| Folder | Restaurant Role | Job Portal Example |
|--------|----------------|-------------------|
| **controller** | Waiters (take orders, serve food) | Handle HTTP requests (POST /jobs) |
| **service** | Chefs (cook food, business logic) | Validate job data, apply business rules |
| **repository** | Warehouse Manager (store/fetch supplies) | Save/fetch from PostgreSQL |
| **model** | Menu Items (what's on the menu) | Job, User, Application tables |
| **dto** | Order Slips (format of orders) | JSON request/response structure |
| **config** | Restaurant Rules (opening hours, dress code) | Security, CORS, DB connection |
| **exception** | Complaint Manager (handle angry customers) | Return proper error messages |

---

## 🛠️ **STEP 4: CREATE FOLDER STRUCTURE**

**Right-click on `com.jobportal.backend` in VS Code → New Folder:**

Create these folders (one by one):
1. `controller`
2. `service`
3. `repository`
4. `model`
5. `dto`
6. `config`
7. `exception`
8. `util`

**Your structure should now look like:**
```
com.jobportal.backend/
├── JobportalBackendApplication.java
├── controller/
├── service/
├── repository/
├── model/
├── dto/
├── config/
├── exception/
└── util/
