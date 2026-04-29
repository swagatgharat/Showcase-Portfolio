# Portfolio CMS

A comprehensive, production-ready full-stack portfolio management system designed to showcase professional work, skills, and experience. This application features a robust administrative dashboard for content management, modern frontend aesthetics, and high-level security implementations.

## 1. Project Overview

**Portfolio CMS** is more than just a static website; it's a dynamic Content Management System (CMS) tailored for developers and creative professionals. It allows users to manage their professional identity through a secure admin interface, ensuring that their portfolio is always up-to-date without touching a single line of code.

### Problem Solved
Static portfolios are difficult to maintain and often become outdated. This project solves the problem of content staleness by providing a centralized dashboard to update projects, skills, and professional experience in real-time.

### Target Users
*   **Job Seekers / Developers**: To showcase their work in a premium, high-performance environment.
*   **Recruiters**: To view a clean, organized, and detailed breakdown of a candidate's technical expertise and project history.
*   **Admins (Portfolio Owners)**: To manage all site content through a secure, authenticated dashboard.

---

## 2. Core Features

### Frontend (Visitor Experience)
*   **Interactive Hero Section**: Engaging introduction with dynamic text and animations.
*   **Project Showcase**: Filterable and detailed project cards with tech stack icons and live/source links.
*   **Experience Timeline**: A vertical timeline illustrating professional growth and key milestones.
*   **Skill Matrix**: Categorized technical skills with visual level indicators.
*   **Dynamic Contact Form**: Integrated with Nodemailer and EmailJS for seamless communication.
*   **Premium Aesthetics**: Powered by AOS (Animate On Scroll), Framer Motion, and Lottie animations for a high-end feel.
*   **Terminal-Style Boot Sequence**: A unique loading experience that synchronizes with the application's branding.

### Admin Dashboard (Content Management)
*   **Role-Based Access**: Secure login for the site owner to manage content.
*   **Hero Management**: Update taglines, introductions, and social links.
*   **About Section Editor**: Full control over bio and personal details.
*   **Project CRUD**: Create, Read, Update, and Delete project entries, including image uploads via ImageKit.
*   **Skill Management**: Add or remove technical skills across various categories.
*   **Experience Manager**: Update work history, company details, and descriptions.
*   **Loading Screen Config**: Manage the terminal-style welcome messages.

---

## 3. Tech Stack

### Frontend
*   **Framework**: [React 19](https://react.dev/)
*   **Build Tool**: [Vite](https://vitejs.dev/)
*   **Styling**: [Tailwind CSS 4](https://tailwindcss.com/), [Material UI (MUI)](https://mui.com/)
*   **Animations**: [Framer Motion](https://www.framer.com/motion/), [AOS](https://michalsnik.github.io/aos/), [Lottie](https://lottiefiles.com/)
*   **State & Routing**: React Router DOM 7
*   **HTTP Client**: Axios

### Backend
*   **Runtime**: [Node.js](https://nodejs.org/)
*   **Framework**: [Express.js 5](https://expressjs.com/)
*   **Database**: [MongoDB](https://www.mongodb.com/) via [Mongoose](https://mongoosejs.com/)
*   **Logging**: [Pino](https://getpino.io/) with Pino-Pretty

### Security & Utilities
*   **Authentication**: JSON Web Tokens (JWT) with secure HTTP-only cookies.
*   **Security Headers**: [Helmet.js](https://helmetjs.github.io/)
*   **CSRF Protection**: [csrf-csrf](https://github.com/PsycReal/csrf-csrf) (Double Submit Cookie pattern).
*   **Rate Limiting**: Express Rate Limit
*   **Media Storage**: [ImageKit.io](https://imagekit.io/)
*   **Emailing**: [Nodemailer](https://nodemailer.com/)

---

## 4. Architecture Overview

### Frontend Structure
The frontend follows a modular, component-based architecture:
*   **Pages**: Separated into `admin` (dashboard) and `common` (public facing).
*   **Components**: Reusable UI elements (Navbar, Footer, Cards).
*   **API Layer**: Centralized Axios instances with interceptors for auth and CSRF handling.
*   **Hooks/Utils**: Custom logic for state management and formatting.

### Backend Structure
The backend implements a Clean Architecture pattern:
*   **Routes**: Defines API endpoints and applies middleware.
*   **Controllers**: Handles request/response logic.
*   **Services**: Encapsulates business logic and database interactions.
*   **Models**: Mongoose schemas for data persistence.
*   **Middleware**: Centralized error handling, authentication, and security checks.

### Authentication Flow
1. Admin logs in via `/api/auth/login`.
2. Server validates credentials and issues a JWT stored in an `admin_token` HTTP-only cookie.
3. A CSRF token is generated and sent via `/api/csrf-token`.
4. Subsequent requests include both the cookie and the CSRF header for dual-layer security.

---

## 5. Folder Structure

```text
.
├── backend/                # Express Server
│   ├── src/
│   │   ├── config/         # DB and environment config
│   │   ├── controllers/    # Request handlers
│   │   ├── middleware/     # Auth, CSRF, Rate Limiting
│   │   ├── models/         # Mongoose Schemas
│   │   ├── routes/         # API Route definitions
│   │   ├── services/       # Business logic
│   │   ├── utils/          # Logger, Initializers
│   │   └── validations/    # Joi/Express-validator schemas
│   └── .env                # Server environment variables
├── frontend/               # React Application
│   ├── src/
│   │   ├── api/            # API integration layer
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/          # Admin and Common sections
│   │   ├── utils/          # Helpers
│   │   └── App.jsx         # Main routing and entry
│   └── vite.config.js      # Vite configuration
├── package.json            # Root scripts for monolith deployment
└── README.md
```

---

## 6. Environment Variables

Create a `.env` file in the `backend/` directory with the following variables:

```env
# Server Config
PORT=5000
NODE_ENV=production

# Database
MONGO_URI=your_mongodb_uri

# Security
JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=7d
CSRF_SECRET=your_csrf_secret
ALLOWED_ORIGINS=http://localhost:5173

# Admin Initial Credentials
ADMIN_USERNAME=admin@example.com
ADMIN_PASSWORD=your_secure_password

# ImageKit (Media)
IMAGEKIT_PUBLIC_KEY=your_public_key
IMAGEKIT_PRIVATE_KEY=your_private_key
IMAGEKIT_URL_ENDPOINT=your_url_endpoint

# Email Config (Nodemailer)
EMAIL_SERVICE=gmail
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password
RECEIVER_EMAIL=your_email@gmail.com
```

---

## 7. Installation & Setup

### Prerequisites
*   Node.js (v18+)
*   MongoDB Atlas account
*   ImageKit account

### Step-by-Step Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-repo/portfolio.git
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
   cd ../frontend
   npm install
   # Create .env for VITE_API_URL if needed
   npm run dev
   ```

4. **Database Initialization**
   On the first run, the backend will automatically create the admin user and seed initial data from `src/utils/init*.js` using your `.env` credentials.

---

## 8. Security Implementation

*   **Authentication**: JWTs are stored in `HttpOnly` and `Secure` cookies to prevent XSS attacks.
*   **CSRF Protection**: Implemented via `csrf-csrf` using the Double Submit Cookie pattern, synchronized with the JWT session.
*   **Rate Limiting**: Prevents brute-force attacks on sensitive routes (auth, contact) using `express-rate-limit`.
*   **Helmet**: Sets various HTTP headers (CSP, HSTS, etc.) to harden the server.
*   **Input Validation**: All incoming data is sanitized and validated using `Joi` and `express-validator` to prevent injection attacks.
*   **Ownership Checks**: Admin routes are protected by `authMiddleware` ensuring only the authenticated owner can modify data.

---

## 9. API Documentation

| Method | Endpoint | Description | Auth Required |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/health` | Service health status | No |
| `POST` | `/api/auth/login` | Admin login | No |
| `POST` | `/api/auth/logout` | Clear auth cookies | Yes |
| `GET` | `/api/projects` | Fetch all projects | No |
| `POST` | `/api/projects` | Create new project | Yes |
| `PUT` | `/api/projects/:id` | Update project | Yes |
| `DELETE` | `/api/projects/:id` | Delete project | Yes |
| `POST` | `/api/contact` | Send contact email | No |
| `POST` | `/api/upload` | Upload image to ImageKit | Yes |
| `GET` | `/api/csrf-token` | Generate CSRF token | No |

---

## 10. Deployment Instructions

### Deployment to Render (Monolith Mode)
The project is configured for a monolith deployment where the backend serves the built frontend.

1.  **Build Command**:
    ```bash
    npm run build
    ```
    *(This runs the root build script: builds frontend, installs backend deps)*
2.  **Start Command**:
    ```bash
    npm start
    ```
3.  **Environment Variables**: Ensure all variables from Section 6 are added to the Render dashboard.
4.  **Static Files**: The backend serves `frontend/dist` in production mode.

---

## 11. Future Improvements

*   **AI Chatbot Integration**: An AI assistant to answer questions about my skills and experience.
*   **Blog Module**: A section to share technical articles and insights.
*   **Dark/Light Mode**: Dynamic theme switching for better accessibility.
*   **Analytics Dashboard**: Track visitor engagement and project views.

---

## 12. Contributing Guidelines

1.  **Fork** the repository.
2.  Create a **Feature Branch** (`git checkout -b feature/AmazingFeature`).
3.  **Commit** your changes (`git commit -m 'Add some AmazingFeature'`).
4.  **Push** to the branch (`git push origin feature/AmazingFeature`).
5.  Open a **Pull Request**.

**Code Style**: Please follow the camelCase naming convention for files and variables as established in the project guidelines.

---
*Designed & developed [Swagat Gharat](https://github.com/swagatgharat)*
