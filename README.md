# Auto-Testing-Website (TestPilot)

A web platform for automated website/web-app testing. Users upload their source code (`.zip`) or connect a GitHub repository, and the system automatically analyzes the codebase, generates test scripts, runs them with Playwright, and returns a visual report on the website's health.

## Key Features

- Upload source code as a `.zip` or select a repository/branch from GitHub
- Choose test type: UI Testing, API Testing, Functional Testing
- Near real-time test progress tracking (pipeline status, completion %, error logs)
- Result reports: website health score (out of 100), pass/fail test counts, run duration, and a list of detected errors with severity and affected page/file
- Account management, test run history, credit usage, billing/payments
- Admin dashboard, support chat, and marketing pages (pricing, docs, roadmap, blog...)

## Architecture

```
├── backend/     # Node.js + Express API server and AI pipeline
├── frontend/    # React SPA (CRA/craco)
└── docker-compose.yml
```

### Backend (`backend/`)

- **API** (`src/api`): `auth`, `billing`, `chat`, `contact`, `github`, `status`, `test`, `admin`
- **Pipeline** (`src/pipeline`): an AI pipeline that processes source code through sequential stages in `src/pipeline/stages`:
  `routeScanner → detector → analyzer → planner → coder → validator → executor → debugger → filter → recommender → reporter`
- Runs actual tests with **Playwright**
- Connects to MongoDB / Supabase, handles payments via Stripe, uploads to S3, sends email via Nodemailer

### Frontend (`frontend/`)

- React 19 + React Router, UI built on Radix UI + Tailwind CSS
- Main pages: Landing, Login/Register, Dashboard, Test Runner, Test Progress, Test Report, Profile, Billing, Admin, and content pages (Docs, Pricing, Blog, Roadmap...)

## Running the Project

### With Docker Compose

```bash
docker compose up --build
```

- Backend: `http://localhost:5000`
- Frontend: `http://localhost:3000`

Configure backend environment variables in `backend/.env` (do not commit this file).

### Manually

```bash
# Backend
cd backend
npm install
npx playwright install --with-deps
npm run dev      # or: npm start

# Frontend
cd frontend
yarn install
yarn start
```

### Deploying to a Server (EC2)

Get the EC2 instance's Public IPv4 → point your DNS (PA Vietnam) to it → SSH into the EC2 instance → run `setup.sh` (installs Node, PM2, Nginx, Certbot, clones the repo, installs dependencies, and configures HTTPS).

## Tech Stack

- **Frontend**: React, React Router, Tailwind CSS, Radix UI, Framer Motion
- **Backend**: Node.js, Express, MongoDB, Supabase, Stripe, WebSocket (ws)
- **Testing**: Playwright
- **Infrastructure**: Docker, Nginx, PM2, Let's Encrypt (Certbot)
