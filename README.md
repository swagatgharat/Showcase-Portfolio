# Portfolio CMS

A comprehensive, production-ready full-stack portfolio management system designed to showcase professional work, skills, and experience. This application features a robust administrative dashboard for content management, modern frontend aesthetics, and high-level security implementations.

## Links

- **Live Demo**: [https://swagat-portfolio.onrender.com](https://swagat-portfolio.onrender.com)

## Image

<img width="3150" height="2205" alt="Kokuyo" src="https://github.com/user-attachments/assets/95891b59-15a4-41c5-8e9e-6638d1975f8f" />

## 1. Project Overview

**Portfolio CMS** is more than just a static website; it's a dynamic Content Management System (CMS) tailored for developers and creative professionals. It allows users to manage their professional identity through a secure admin interface, ensuring that their portfolio is always up-to-date without touching a single line of code.

### Problem Solved

Static portfolios are difficult to maintain and often become outdated. This project solves the problem of content staleness by providing a centralized dashboard to update projects, skills, and professional experience in real-time.

### Target Users

- **Job Seekers / Developers**: To showcase their work in a premium, high-performance environment.
- **Recruiters**: To view a clean, organized, and detailed breakdown of a candidate's technical expertise and project history.
- **Admins (Portfolio Owners)**: To manage all site content through a secure, authenticated dashboard.

---

## 2. Core Features

- **Interactive UI/UX**: Engaging hero section, premium animations (Framer Motion, AOS, Lottie), and a terminal-style boot sequence.
- **Project Showcase**: Filterable, dynamically managed project showcases synchronized with GitHub and featuring ImageKit uploads.
- **Professional Timeline**: An interactive timeline detailing career milestones, work experience, and educational background.
- **Content Management (CMS)**: Secure, authenticated admin dashboard to update all sections, skills, loading screens, and projects in real-time.
- **Dynamic Contact System**: A contact form connected to Gmail API (OAuth2) with built-in rate-limiting and security checks.

---

## 3. Tech Stack

- **Framework**: [Next.js 16 (App Router)](https://nextjs.org/) / [React 19](https://react.dev/)
- **Language**: [TypeScript](https://www.typescriptlang.org/)
- **Runtime**: [Node.js](https://nodejs.org/) / [Express.js 5](https://expressjs.com/)
- **Database**: [MongoDB](https://www.mongodb.com/) via [Mongoose](https://mongoosejs.com/)
- **Styling**: [Tailwind CSS 4](https://tailwindcss.com/) / [Material UI (MUI) 9](https://mui.com/)
- **Storage**: [ImageKit.io](https://imagekit.io/) for cloud media storage
- **Mailing**: Gmail API via OAuth2

---

## 4. Architecture Overview

### Frontend Structure

The frontend follows a modular App Router architecture in Next.js:

- **App Routes**: Located in `src/app`, handling routing for the public layout and pages, as well as the protected admin dashboard (`/admin`).
- **Components**: Reusable UI elements, located in `src/components`, separated into layouts/sections and admin components.
- **API Layer**: Centralized Axios instances located in `src/api` with Bearer auth support and automatic token refresh interceptors.
- **Utils**: Custom utility helpers for client-side functionality.

### Backend Structure

The backend implements a Clean Architecture pattern:

- **Routes**: Defines API endpoints and applies middleware.
- **Controllers**: Handles request/response logic.
- **Services**: Encapsulates business logic and database interactions.
- **Models**: Mongoose schemas for data persistence.
- **Middleware**: Centralized error handling, authentication, and security checks.

### Authentication Flow

1. Admin logs in via `POST /api/auth/login` → receives `accessToken` (15m) in response body and `refreshToken` (7d) in an `httpOnly` cookie.
2. Access token is held in memory; refresh token is managed automatically by the browser via the cookie (inaccessible to JS).
3. Admin API requests send `Authorization: Bearer <accessToken>`.
4. On 401 with error code `TOKEN_EXPIRED`, the client calls `POST /api/auth/refresh` (passing no body; the cookie is sent automatically) to receive a new access token and retries the request.
5. Logout revokes the refresh token server-side via `POST /api/auth/logout` and clears the cookie.

---

## 5. Folder Structure

```text
.
├── backend/                # Express Server
│   ├── src/
│   │   ├── config/         # DB and environment config
│   │   ├── controllers/    # Request handlers
│   │   ├── middleware/     # Auth, Rate Limiting
│   │   ├── models/         # Mongoose Schemas
│   │   ├── routes/         # API Route definitions
│   │   ├── services/       # Business logic
│   │   ├── utils/          # Logger, Initializers
│   │   └── validations/    # Joi/Express-validator schemas
│   └── .env                # Server environment variables
├── frontend-next/          # Next.js Application
│   ├── src/
│   │   ├── api/            # API integration layer
│   │   ├── app/            # App Router routes and pages
│   │   ├── components/     # Reusable UI components
│   │   └── utils/          # Helpers
│   ├── next.config.ts      # Next.js configuration
│   └── tsconfig.json       # TypeScript configuration
├── package.json            # Root scripts for monolith deployment
└── README.md
```

---

## 6. Environment Variables

### Backend Configuration

Create a `.env` file in the `backend/` directory with the following variables:

```env
# Server Config
PORT=5000
NODE_ENV=production

# Database
MONGO_URI=your_mongodb_uri

# Security
JWT_ACCESS_SECRET=your_access_secret
JWT_REFRESH_SECRET=your_refresh_secret
ALLOWED_ORIGINS=http://localhost:3000

# Admin Initial Credentials
ADMIN_USERNAME=admin@example.com
ADMIN_PASSWORD=your_secure_password

# ImageKit (Media)
IMAGEKIT_PUBLIC_KEY=your_public_key
IMAGEKIT_PRIVATE_KEY=your_private_key
IMAGEKIT_URL_ENDPOINT=your_url_endpoint

# Email (Gmail API + OAuth2 — works on Render; no SMTP)
GMAIL_USER=your_email@gmail.com
OAUTH_CLIENT_ID=your_google_client_id
OAUTH_CLIENT_SECRET=your_google_client_secret
OAUTH_REFRESH_TOKEN=your_refresh_token
RECEIVER_EMAIL=your_email@gmail.com
```

### Frontend Configuration

Create a `.env.local` file in the `frontend-next/` directory for local development:

```env
# Local Development API URL (Express backend runs on port 5000)
NEXT_PUBLIC_API_URL=http://localhost:5000/api
```

---

## 7. Installation & Setup

### Prerequisites

- Node.js (v18+)
- MongoDB Atlas account
- ImageKit account

### Step-by-Step Setup

1. **Clone the Repository**

   ```bash
   git clone https://github.com/swagatgharat/Portfolio.git
   cd portfolio
   ```

2. **Backend Setup**

   ```bash
   cd backend
   npm install
   # Create .env and fill in variables
   npm run dev
   ```

3. **Frontend Setup**

   ```bash
   cd ../frontend-next
   npm install
   # Create .env.local and add NEXT_PUBLIC_API_URL
   npm run dev
   ```

4. **Database Initialization**
   On the first run, the backend will automatically create the admin user and seed initial data from `src/utils/init*.js` using your `.env` credentials.

---

## 8. Security Implementation

- **Authentication**: Short-lived access JWT in memory; refresh JWT in a secure, `httpOnly`, `SameSite=Lax` cookie with server-side revocation on logout.
- **Authorization**: `Authorization: Bearer` header on all protected admin routes (`authMiddleware`).
- **Rate Limiting**: Prevents brute-force attacks on sensitive routes (auth, contact) using `express-rate-limit`.
- **Helmet**: Sets various HTTP headers (CSP, HSTS, etc.) to harden the server.
- **Input Validation**: All incoming data is sanitized and validated using `Joi` and `express-validator` to prevent injection attacks.
- **Ownership Checks**: Admin routes are protected by `authMiddleware` ensuring only the authenticated owner can modify data.

---

## 9. API Documentation

| Method   | Endpoint               | Description                                             | Auth Required |
| :------- | :--------------------- | :------------------------------------------------------ | :------------ |
| `GET`    | `/api/health`          | Service health status                                   | No            |
| `POST`   | `/api/auth/login`      | Admin login (returns access token, sets refresh cookie) | No            |
| `POST`   | `/api/auth/refresh`    | Rotate access token via refresh cookie                  | No            |
| `POST`   | `/api/auth/logout`     | Revoke refresh session                                  | Yes           |
| `GET`    | `/api/auth/check-auth` | Verify current session                                  | Yes           |
| `GET`    | `/api/projects`        | Fetch all projects                                      | No            |
| `POST`   | `/api/projects`        | Create new project                                      | Yes           |
| `PUT`    | `/api/projects/:id`    | Update project                                          | Yes           |
| `DELETE` | `/api/projects/:id`    | Delete project                                          | Yes           |
| `PUT`    | `/api/projects/:id/github-sync` | Sync project details with GitHub              | Yes           |
| `POST`   | `/api/projects/github-details` | Fetch GitHub repository details without saving | Yes           |
| `POST`   | `/api/contact`         | Send contact email                                      | No            |
| `POST`   | `/api/upload`          | Upload image to ImageKit                                | Yes           |

---

## 10. Deployment Instructions

### Deployment to Render (Monolith Mode)

The project is configured for a monolith deployment where the backend acts as a reverse proxy, forwarding all non-API traffic to the Next.js production server running on port 3000.

1.  **Build Command**:
    ```bash
    npm run build
    ```
    _(This runs the root build script: installs root dependencies, installs frontend dependencies, builds the Next.js app, and installs backend dependencies)_
2.  **Start Command**:
    ```bash
    npm start
    ```
    _(This starts both the Next.js production server and the Express backend concurrently using `concurrently`)_
3.  **Environment Variables**: Ensure all variables from Section 6 (both backend `.env` and frontend `NEXT_PUBLIC_API_URL`) are added to the Render dashboard.
4.  **Proxy Behavior**: The backend forwards frontend traffic to `http://127.0.0.1:3000` in production mode.

---

## 11. Future Improvements

- **AI Chatbot Integration**: An AI assistant to answer questions about my skills and experience.
- **Blog Module**: A section to share technical articles and insights.
- **Dark/Light Mode**: Dynamic theme switching for better accessibility.
- **Analytics Dashboard**: Track visitor engagement and project views.

---

## 12. Contributing Guidelines

1.  **Fork** the repository.
2.  Create a **Feature Branch** (`git checkout -b feature/AmazingFeature`).
3.  **Commit** your changes (`git commit -m 'Add some AmazingFeature'`).
4.  **Push** to the branch (`git push origin feature/AmazingFeature`).
5.  Open a **Pull Request**.

**Code Style**: Please follow the camelCase naming convention for files and variables as established in the project guidelines.

---

_Designed & developed [Swagat Gharat](https://github.com/swagatgharat)_
