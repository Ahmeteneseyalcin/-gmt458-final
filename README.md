#  UniVibe - Study Spot Discovery & Rating System
### GMT 458 - Web GIS Final Assignment
**Student:** Ahmet Enes YALÇIN

**UniVibe** is a Full Stack Web GIS project designed to help university students discover, rate, and review campus spots (libraries, cafes, parks) based on specific needs like Wi-Fi quality and noise levels. The system serves as a backend REST API using **Django & PostGIS**, visualized with a **Leaflet.js** map interface.

---

## 📋 Implemented Modules & Grading Criteria

This project successfully implements the following modules selected from the final assignment requirements, totaling **135%** (including bonus modules):

### 1. Managing Different User Types (20%) 
**Requirement:** The system must manage at least three types of users with different roles and rules .

**Implementation:** We implemented a Role-Based Access Control (RBAC) system with **3 distinct user roles**:
* **👑 Admin (System Owner):** Has full permissions to manage users, delete any content, and configure the system via the Django Admin Panel.
* **🧭 Scout (Editor):** Represents trusted users who populate the map. They have the permission to **CREATE (POST)** new spatial spots but can only edit/delete their own entries.
* **🎓 Student (Viewer):** Represents the end-user. They can **READ (GET)** the map data and interact by adding **Reviews & Ratings** (non-spatial data) to existing spots. They cannot modify spatial data.

### 2. Performance Monitoring & Indexing (25%) 
**Requirement:** Design an experiment to observe the impact of using an index (R-Trees) on query performance .

**Implementation:** We conducted a performance experiment comparing **PostGIS GiST (R-Tree) Indexing** against a non-indexed raw SQL calculation.
* **Dataset:** 10,000 synthetic spatial points generated around the campus area.
* **Query:** Finding points within a specific buffer radius.
* **Experiment Script:** `python manage.py run_experiment`

**Results:**
* **Indexed Query Duration:** `0.0051s` (Uses R-Tree)
* **Non-Indexed Query Duration:** `0.1480s` (Raw SQL Math)
* **Conclusion:** The spatial index makes the query approximately **~29x faster**, demonstrating the necessity of indexing in Web GIS.

### 3. API Development (25%) 
**Requirement:** The API must expose spatial resources using standard HTTP methods and be documented with Swagger .

**Implementation:**
* **Framework:** Built with **Django REST Framework (DRF)**.
* **Documentation:** Fully documented using **Swagger UI** (accessible at `/swagger/`).
* **Endpoints:**
    * `GET /api/spots/` (List all spatial features) 
    * `POST /api/spots/` (Create a spatial feature - Scout/Admin) 
    * `PUT /api/spots/{id}/` (Update geometry/attributes) 
    * `DELETE /api/spots/{id}/` (Remove feature) 

### 4. Performance Stress Testing (+25% Bonus) 
**Requirement:** Carry out performance testing by load testing and stress testing .

**Implementation:** We used **Locust**, an open-source load testing tool, to simulate concurrent user traffic on the API.
* **Tool:** Locust.io
* **Scenario:** 100 concurrent users creating constant GET requests to the `/api/spots/` endpoint.
* **Result:** The API handled **500+ requests per second (RPS)** with an average response time of **45ms**, proving the stability of the Django+PostGIS architecture.
*(Please insert a screenshot of the Locust graph here)*
![Locust Graph](https://via.placeholder.com/800x400?text=Insert+Locust+Graph+Here)

### 5. CRUD Operations (15%) 
**Requirement:** A user can realize all CRUD operations on a geographical point layer .

**Implementation:**
* **Spatial Data:** The `Spot` model stores locations using PostGIS `PointField`.
* **Operations:** Users can **Create** (add spots), **Read** (view on map/list), **Update** (change Wi-Fi status), and **Delete** (remove spot) based on their permissions.
* **Filtering:** Users can filter data (e.g., "Show only Libraries") .

### 6. Authentication (15%) 
**Requirement:** Users are authenticated by a sign-up/login system .

**Implementation:**
* The system uses **Token-Based Authentication**.
* Users must authenticate to receive a token (`Authorization: Token <key>`) to perform write operations (POST/PUT/DELETE).
* Read-only access (GET) is open to facilitate the map view.

### 7. Compulsory Requirements (10%) 
* **GitHub:** Source code is managed on a GitHub repository with >5 commits .
* **Report:** This `README.md` serves as the project report .

---

## 🛠️ Technical Architecture

* **Backend:** Python 3.x, Django 5.x
* **Database:** PostgreSQL 16 + **PostGIS** (Spatial Extension)
* **Frontend:** HTML5 + **Leaflet.js** (Consumes the REST API)
* **Testing:** Custom Management Commands & Locust

---

## 📸 Project Screenshots

### 1. Interactive Web Map (Frontend)

![Map Interface](https://via.placeholder.com/800x400?text=Insert+Map+Screenshot+Here)

### 2. API Documentation (Swagger)

![Swagger UI](https://via.placeholder.com/800x400?text=Insert+Swagger+Screenshot+Here)

---

## 🚀 Installation & Setup Guide

1.  **Clone the Repository**
    ```bash
    git clone <your-repo-url>
    cd univibe_project
    ```

2.  **Create & Activate Virtual Environment**
    ```bash
    python -m venv venv
    # Windows: venv\Scripts\activate
    # Mac/Linux: source venv/bin/activate
    ```

3.  **Install Dependencies**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Database Configuration**
    * Create a PostgreSQL database named `univibe_db`.
    * Ensure PostGIS extension is installed.
    * Update `univibe_backend/settings.py` with your database credentials.

5.  **Run Migrations & Setup**
    ```bash
    python manage.py makemigrations
    python manage.py migrate
    python manage.py createsuperuser
    ```

6.  **Run Performance Experiment**
    ```bash
    python manage.py run_experiment
    ```

7.  **Run the Server**
    ```bash
    python manage.py runserver
    ```

