# AI Interview Platform

![MERN Stack](https://img.shields.io/badge/Stack-MERN-blue?style=flat-square&logo=mongodb)
![React](https://img.shields.io/badge/Frontend-React%20%2B%20Vite-61DAFB?style=flat-square&logo=react)
![NodeJS](https://img.shields.io/badge/Backend-Node.js-339933?style=flat-square&logo=node.js)
![Google GenAI](https://img.shields.io/badge/AI-Google%20GenAI%20%28Gemini%29-FFCA28?style=flat-square&logo=google)

An advanced, end-to-end full-stack MERN application that leverages the **Google GenAI API (Gemini 3 Flash)** to help job seekers crack their dream role. The platform allows users to upload their resumes or self-descriptions alongside a target Job Description to automatically generate an extensive, AI-powered interview preparation plan, mock questions, and highly tailored ATS-friendly resumes.

## 🚀 Features

- **Robust Authentication Flow**: Secure JWT-based authentication with cookie HTTP-only parsing, and bcrypt hashed passwords.
- **AI Interview Strategy Reports**:
  - **Match Scoring**: Calculates a percentage matrix on how well the candidate's profile matches the job requirements.
  - **Skill Gaps Analysis**: Identifies exact skill weaknesses and flags them with priority/severity scores.
  - **Mock Questions Engine**: Generates targeted Technical & Behavioral questions with expected intentions and "how to answer" guides.
  - **Preparation Plan**: Gives a day-by-day roadmap pointing exactly what to practice to close skill gaps.
- **Smart ATS Resume Generator**:
  - Automatically reconstructs user resumes strictly tailored to the job description constraints via the Gemini Prompt schema.
  - Converts dynamically generated structured HTML to a downloadable A4 PDF using **Puppeteer**.
- **Resume Parsing**: Parses `.pdf` files extracting inner raw text using `pdf-parse`.

---

## 🛠️ Technology Stack

### Frontend
- **Framework**: React 19 (via Vite)
- **Routing**: React Router v7
- **Styling**: SCSS (Sass) for customized styling.
- **HTTP Client**: Axios

### Backend
- **Runtime**: Node.js & Express.js
- **Database**: MongoDB (via Mongoose)
- **Generative AI SDK**: `@google/genai` bridging structured prompt parsing mapped manually via `zod` and `zod-to-json-schema`.
- **Parsing & Generation**: 
  - `pdf-parse` (Converting PDF text buffers to string format)
  - `puppeteer` (Headless browser rendering HTML code representations as physical PDF downloads)
- **Authentication**: `jsonwebtoken`, `bcryptjs`, `cookie-parser`
- **File Upload Handling**: `multer`

---

## 📂 Architecture Overview

```text
NextHire.ai/
├── Backend/                 # Express backend server
│   ├── src/
│   │   ├── config/          # Database configuration (MongoDB)
│   │   ├── controllers/     # Route business logic (Auth, Interview)
│   │   ├── middlewares/     # JWT Auth check & Multer File parser
│   │   ├── models/          # Mongoose Schemas (User, InterviewReport)
│   │   ├── routes/          # API endpoint routes
│   │   └── services/        # Third-party integrations (GenAI & Puppeteer)
│   ├── app.js               # Express application pipeline
│   └── server.js            # Node startup script
│
└── Frontend/                # Vite React App
    ├── src/
    │   ├── features/        # Feature-based architectures (Auth, Interview)
    │   ├── style/           # SCSS Styling abstractions
    │   ├── App.jsx          # Root Component & Context Providers
    │   └── app.routes.jsx   # Route descriptors (Protected Routes)
```

---

## 🔧 Installation & Setup

### Prerequisites
- [Node.js](https://nodejs.org/en) (v18+)
- [MongoDB](https://www.mongodb.com/try/download/community) Instance (Local or Atlas)
- [Google Gemini API Key](https://aistudio.google.com/app/apikey)

### 1. Clone the repository
```bash
git clone https://github.com/your-username/NextHire.ai.git 
cd NextHire.ai
```

### 2. Backend Setup
Navigate into the backend directory and install dependencies:
```bash
cd Backend
npm install
```

Create a `.env` file in the `Backend/` root folder and configure the following variables:
```env
PORT=3000
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
GOOGLE_GENAI_API_KEY=your_gemini_api_key
```

Start the backend server in dev mode:
```bash
npm run dev
```
*The backend should load successfully on `http://localhost:3000`.*

### 3. Frontend Setup
Navigate into the frontend directory and install dependencies:
```bash
cd ../Frontend
npm install
```

Start the Vite development server:
```bash
npm run dev
```
*The frontend should now be running on `http://localhost:5173`.*

---

## 🔌 Core API Endpoints

### Authentication (`/api/auth`)
- `POST /register` – Register a new candidate.
- `POST /login` – Login & issue JWT token.
- `GET /logout` – Discard session.
- `GET /get-me` – Retrieve authorized user details.

### Interview Generation (`/api/interview`)
- `POST /` – Generate interview roadmap logic, evaluating user `resume` (PDF buffer over Multer) and Job Description matrices using Google's `gemini-3-flash-preview`.
- `GET /` – Fetch all historical interview strategies for a user.
- `GET /report/:interviewId` – Get detailed JSON metadata structure per interview iteration.
- `POST /resume/pdf/:interviewReportId` – Prompt Gemini to render custom ATS Resume HTML structures; converts it into a `Buffer` stream back to the UI triggering a direct PDF attachment download via `puppeteer`.

---

## 📜 License
This project is licensed under the ISC License.
