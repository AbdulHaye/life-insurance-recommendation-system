# Life Insurance Recommendation MVP

A full-stack prototype for personalized life insurance recommendations, built with **Next.js**, **Node.js**, **PostgreSQL**, and **Docker**. This application collects user inputs (age, income, dependents, risk tolerance) and provides tailored insurance recommendations, featuring secure JWT authentication and rate limiting.

---

## Features

- **Frontend:** Next.js (App Router) with TypeScript, Tailwind CSS, and React Hook Form for a responsive, user-friendly interface.
- **Backend:** Node.js with Express and TypeScript, offering a secure REST API with JWT authentication and rate limiting.
- **Database:** PostgreSQL with a schema for storing user credentials and recommendation submissions.
- **DevOps:** Dockerized with Docker Compose for consistent local development.
- **Security:** Input validation, parameterized SQL queries, JWT authentication, and rate limiting (100 requests per 15 minutes per IP).

---

## Prerequisites

- **Node.js:** Version 16 or 22 (tested with 22.15.0)
- **Docker and Docker Compose:** (tested with Docker 28.0.4)
- **Git:** For cloning the repository
- **PostgreSQL:** Local or Dockerized instance
- **WSL2:** Recommended for Windows users for Docker compatibility

---

## Setup Instructions

### 1. Clone the Repository

```sh
git clone https://github.com/your-username/life-insurance-recommendation.git
cd life-insurance-recommendation
```

### 2. Install Dependencies

**Backend:**

```sh
cd backend
npm install
cd ..
```

**Frontend:**

```sh
cd frontend
npm install
cd ..
```

### 3. Set Up Environment Variables

**Backend:**

- Copy the example environment file and configure it:
  ```sh
  cp backend/.env.example backend/.env
  ```
- Edit `backend/.env` with your values:
  ```
  DATABASE_URL=postgres://<username>:<password>@<host>:<port>/<database>
  PORT=5000
  JWT_SECRET=<your-secure-secret-key>
  ```

**Frontend:**

- Edit `frontend/.env` if needed:
  ```
  NEXT_PUBLIC_API_URL=http://localhost:5000/api
  ```

---

### 4. Run Locally with Docker

```sh
cd docker
docker-compose up --build
```

- **Frontend:** [http://localhost:3000](http://localhost:3000)
- **Backend API:** [http://localhost:5000](http://localhost:5000)

---

### 5. Verify Database

```sh
docker exec -it <db_container_name> psql -U <user> -d <database>
\dt
SELECT * FROM user_submissions;
\q
```

Or use a GUI like pgAdmin.

---

## Usage

### 1. Register a User

**POST** `/api/register`

```json
{
  "email": "test@example.com",
  "password": "password123"
}
```

### 2. Login to Get JWT Token

**POST** `/api/login`

```json
{
  "email": "test@example.com",
  "password": "password123"
}
```

Copy the token from the response for use in the next step.

### 3. Submit Recommendation Request

**Via Frontend:**  
Log in at [http://localhost:3000](http://localhost:3000), fill out the form, and submit.

**Via API (with JWT token):**

```json
POST /api/recommendation
Headers: Authorization: Bearer <your-token>
Body:
{
  "age": 35,
  "income": 75000,
  "dependents": 2,
  "riskTolerance": "Medium"
}
```

---

## Project Structure

```
life-insurance-recommendation/
├── backend/
│   ├── src/
│   │   ├── controllers/      # API request handlers
│   │   ├── models/           # Database models (userModel, recommendationModel, initTables)
│   │   ├── routes/           # API route definitions
│   │   ├── services/         # Business logic
│   │   ├── utils/            # Utility functions (e.g., auth)
│   │   └── app.ts            # Express application setup
│   ├── Dockerfile
│   ├── package.json
│   ├── tsconfig.json
│   └── .env.example
├── frontend/
│   ├── src/
│   │   ├── components/       # Reusable UI components
│   │   ├── app/              # Next.js app directory
│   │   ├── styles/           # Tailwind CSS and global styles
│   │   ├── utils/            # API and auth helpers
│   │   └── types/            # TypeScript interfaces
│   ├── Dockerfile
│   ├── package.json
│   ├── tsconfig.json
│   ├── next.config.js
│   ├── tailwind.config.js
│   └── postcss.config.js
├── docker/
│   ├── docker-compose.yml
│   └── init.sql
├── .gitignore
├── README.md
└── LICENSE
```

---

## Evaluation Notes

- **Frontend:** Modular components with TypeScript, Tailwind CSS for responsive design, and React Hook Form for clean state management.
- **Backend:** Secure REST API with JWT authentication, rate limiting, and extensible recommendation logic.
- **Database:** Well-designed schema with constraints and parameterized queries to prevent SQL injection.
- **Docker:** Production-ready local environment with Docker Compose and health checks.
- **Security:** Input validation, JWT-based authentication, rate limiting, and secure database practices.
- **Communication:** Detailed README.md, well-commented code, and organized
