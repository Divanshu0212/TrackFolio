# Trackfolio: AI-Powered Developer Profile & ATS Platform

![Project Banner](https://via.placeholder.com/1200x400/0D1117/40E0D0?text=Trackfolio)

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-B73BFE?style=for-the-badge&logo=vite&logoColor=FFD62E)

## 📑 Table of Contents
- [Project Overview](#-project-overview)
- [Architecture Overview](#-architecture-overview)
- [Key Features](#-key-features)
- [Tech Stack](#-tech-stack)
- [System Components](#-system-components)
- [Pipeline & Workflow](#-pipeline--workflow)
- [Project Structure](#-project-structure)
- [Installation & Setup](#-installation--setup)
- [Deployment](#-deployment)
- [API Documentation](#-api-documentation)
- [Security Features](#-security-features)
- [Team Structure](#-team-structure)
- [Future Roadmap](#-future-roadmap)
- [Contributors](#-contributors)

## 🎯 Project Overview

Trackfolio is a comprehensive, production-ready platform that combines **portfolio management**, **AI-powered ATS resume analysis**, and **intelligent resume generation** into a unified system. Built with a modern microservices architecture, it leverages cutting-edge technologies including LLMs (Groq AI, Ollama), machine learning, and traditional NLP to provide developers with powerful tools for career advancement.

### What Makes This Project Unique?
- **Microservices Architecture**: Decoupled services for scalability and maintainability
- **Hybrid AI-Powered Analysis**: Combines Groq Llama 3.3 LLM with traditional NLP (spaCy, NLTK, sklearn) for comprehensive resume evaluation
- **Real-time ATS Scoring**: Instant feedback on resume compatibility with job descriptions
- **Coding Profile Analysis**: Automatic detection and evaluation of LeetCode, CodeForces, GitHub profiles
- **Multi-Theme Resume Generation**: AI-generated professional resumes in 3 different styles
- **Enterprise-Grade Security**: PII redaction, malware scanning, rate limiting, and OAuth2.0
- **Cloud-Ready**: Configured for Vercel (frontend/backend) and Railway (Python services)

---

## 🏗️ Architecture Overview

The platform follows a **microservices architecture** with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                                │
│                  ┌────────────────────────┐                         │
│                  │   React Frontend       │                         │
│                  │   (React/Vite)         │                         │
│                  │  - Portfolio Manager   │                         │
│                  │  - Resume Upload       │                         │
│                  │  - ATS Analysis UI     │                         │
│                  └───────────┬────────────┘                         │
└──────────────────────────────┼──────────────────────────────────────┘
                               │
                               │ REST API
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      API GATEWAY / BACKEND                          │
│  ┌───────────────────────────────────────────────────────────┐     │
│  │         Node.js/Express Backend (Central Hub)             │     │
│  │  - Authentication (JWT, OAuth2: Google, GitHub)           │     │
│  │  - Portfolio CRUD (Projects, Experience, Skills, Certs)   │     │
│  │  - Resume Management & Storage (MongoDB/Cloudinary)       │     │
│  │  - ATS Analysis Orchestration                             │     │
│  │  - User Management & Sessions                             │     │
│  │  - Security Middleware (Helmet, Rate Limiting)            │     │
│  └─────────┬──────────────────────────┬──────────────────────┘     │
└────────────┼──────────────────────────┼────────────────────────────┘
             │                          │
             │ HTTP Requests            │ HTTP Requests
             ▼                          ▼
┌──────────────────────────┐  ┌────────────────────────────────────┐
│  ATS ANALYSIS SERVICE    │  │   RESUME GENERATION SERVICE        │
├──────────────────────────┤  ├────────────────────────────────────┤
│  ┌────────────────────┐  │  │  ┌──────────────────────────────┐ │
│  │  ats3.py (FastAPI) │  │  │  │  resume_generator.py (Flask) │ │
│  │  - Groq Llama 3.3  │  │  │  │  - Ollama Mistral Model      │ │
│  │  - NLP Analysis    │  │  │  │  - 3 Resume Themes:          │ │
│  │  - spaCy/NLTK      │  │  │  │    * Modern (Blue)           │ │
│  │  - sklearn ML      │  │  │  │    * Dark Theme              │ │
│  │  - TF-IDF/Cosine   │  │  │  │    * Minimalist B&W          │ │
│  │  - Coding Profile  │  │  │  │  - FPDF Generation           │ │
│  │    Analysis        │  │  │  │  - ZIP Export                │ │
│  └────────────────────┘  │  │  └──────────────────────────────┘ │
└──────────────────────────┘  └────────────────────────────────────┘
             │
             ▼
┌──────────────────────────────────────┐
│      DATA PERSISTENCE LAYER          │
│  ┌────────────────┐  ┌────────────┐ │
│  │  MongoDB       │  │ Cloudinary │ │
│  │  - Users       │  │ - Images   │ │
│  │  - Portfolios  │  │ - Resumes  │ │
│  │  - ATS Reports │  │ - Files    │ │
│  │  - Sessions    │  │            │ │
│  └────────────────┘  └────────────┘ │
└──────────────────────────────────────┘
```

### Service Communication Flow
1. **Client → Backend API**: All user interactions route through Express.js backend
2. **Backend → MongoDB**: User data, portfolios, and ATS analysis reports persistence
3. **Backend → Cloudinary**: Static asset storage (profile images, certificates, project images)
4. **Backend → ATS Service (ats3.py)**: HTTP requests to FastAPI service for resume analysis
5. **Backend → Resume Service**: HTTP requests to Flask service for AI-powered resume generation
6. **ATS Service → Groq API**: LLM inference for intelligent analysis and feedback generation
7. **ATS Service → Coding Platforms**: API calls to fetch profile statistics (LeetCode, CodeForces, etc.)

---

## ✨ Key Features

### 🔐 Authentication & User Management
- **Multi-Strategy Authentication**: JWT tokens, Google OAuth2, GitHub OAuth2
- **Session Management**: Express-session with MongoDB store
- **Password Security**: bcryptjs hashing with salt rounds

### 📊 Portfolio Management
- **Comprehensive Profile**: Projects, work experience, education, skills, certifications
- **File Uploads**: Cloudinary integration with multiple upload configurations
- **Search & Filter**: Text-based search across portfolio items

### 🤖 AI-Powered ATS Analysis
- **Hybrid AI Engine (ats3.py)**:
  - **LLM Component**: Groq Llama 3.3 70B for contextual understanding and intelligent feedback
  - **NLP Component**: spaCy, NLTK for linguistic analysis, tokenization, and NER
  - **ML Component**: sklearn with TF-IDF vectorization and cosine similarity
- **Comprehensive Scoring**:
  - Overall match score (0-100)
  - Category scores (keywords, formatting, experience, skills, education, summary)
  - Missing keywords identification
  - Section-by-section feedback
  - Actionable improvement suggestions
- **Coding Profile Analysis**:
  - Automatic URL detection for LeetCode, CodeForces, GitHub, HackerRank, etc.
  - API integration to fetch profile statistics and ratings
  - Contest participation and problem-solving metrics
  - Overall coding score and job readiness assessment
- **Score Tracking**: Historical score tracking with trend analysis and insights
- **PII Protection**: Automatic PII redaction before processing

### 📄 Resume Generation
- **AI-Enhanced Content**: Ollama Mistral model for professional summaries and bullet points
- **Multiple Templates**: 3 professionally designed themes
- **Batch Download**: ZIP file with all 3 resume variants
- **ATS-Optimized**: Generated resumes follow ATS-friendly formatting

### 🛡️ Enterprise Security
- **Malware Scanning**: Resume file scanning before processing
- **PII Redaction**: Pseudonymization of sensitive data
- **Rate Limiting**: Protection against abuse
- **CORS Configuration**: Secure cross-origin resource sharing
- **Helmet.js**: Security headers and XSS protection
- **Input Validation**: Joi schema validation on all endpoints

---

## 🛠️ Tech Stack

### Frontend
| Technology | Purpose | Version |
|------------|---------|---------|
| React | UI Framework | 19.0.0 |
| Vite | Build Tool & Dev Server | Latest |
| React Router DOM | Client-side Routing | 7.1.3 |
| TailwindCSS | Utility-first CSS | Latest |
| GSAP | Animations | Latest |
| Chart.js | Data Visualization (optional) | Latest |

### Backend (Node.js)
| Technology | Purpose | Version |
|------------|---------|---------|
| Express.js | Web Framework | 5.1.0 |
| MongoDB | Database | via Mongoose 8.16.4 |
| Passport.js | Authentication | 0.7.0 |
| JWT | Token-based Auth | 9.0.2 |
| Cloudinary | Asset Storage | 1.41.3 |
| Multer | File Upload | 1.4.5-lts.2 |
| Helmet | Security Middleware | 8.1.0 |
| Joi | Input Validation | 17.13.3 |
| bcryptjs | Password Hashing | 3.0.2 |

### AI/ML Services (Python)
| Technology | Purpose | Version |
|------------|---------|---------|
| FastAPI | Web Framework | Latest |
| Flask | Resume Gen Server | Latest |
| Groq SDK | LLM API Client | Latest |
| Ollama | Local LLM Runtime | Latest |
| spaCy | NLP Library | Latest |
| NLTK | Text Processing | Latest |
| scikit-learn | ML Algorithms | Latest |
| PyPDF2 | PDF Parsing | Latest |
| python-docx | DOCX Parsing | Latest |
| FPDF | PDF Generation | Latest |

### DevOps & Deployment
| Platform | Service | Purpose |
|----------|---------|---------|
| Vercel | Backend API | Serverless Node.js functions |
| Vercel | Frontend | Static site hosting with SPA routing |
| Railway | Python Services | Container-based Python deployment |
| MongoDB Atlas | Database | Cloud-hosted MongoDB |
| Cloudinary | CDN | Image and file storage |

---

## 🔧 System Components

### 1. Backend API (`/backend`)
**Technology**: Node.js + Express.js
**Port**: 5000 (local), Serverless (production)

**Key Responsibilities:**
- **Authentication**: JWT generation, OAuth2 callbacks, session management
- **User Management**: CRUD operations for user profiles
- **Portfolio CRUD**: Manage projects, experiences, skills, certificates
- **Resume Management**: Upload, retrieve, delete resumes
- **ATS Orchestration**: Route analysis requests to Python services
- **Candidate Management**: Track job applications
- **Security**: Malware scanning, PII redaction, rate limiting

**Key Files:**
- `server.js`: Main entry point, middleware setup
- `routes/`: Express route handlers (auth, user, portfolio, ats, resume)
- `controller/`: Business logic controllers
- `models/`: Mongoose schemas (User, AtsAnalysis, Portfolio, etc.)
- `middleware/`: Auth, file upload, error handling
- `utils/`: Logging, PII redaction, resume parsing, malware scanning
- `config/`: Passport strategies, Cloudinary configs

### 2. ATS Analysis Service (`/ats3.py`)
**Technology**: FastAPI + Groq AI + NLP/ML Libraries
**Port**: 8000 (default)

**Hybrid Analysis Pipeline:**

**Phase 1: Document Processing**
1. **Document Parsing**: Extract text from PDF/DOCX resumes using PyPDF2/python-docx
2. **Text Cleaning**: Normalize whitespace, remove special characters, handle encoding

**Phase 2: NLP Processing**
3. **Tokenization**: NLTK word and sentence tokenization
4. **Named Entity Recognition**: spaCy NER for extracting names, organizations, skills
5. **Stop Word Removal**: Filter common words for meaningful analysis
6. **Keyword Extraction**: Identify technical skills, action verbs, domain keywords

**Phase 3: Statistical Analysis**
7. **TF-IDF Vectorization**: sklearn TfidfVectorizer for resume and job description
8. **Cosine Similarity**: Calculate semantic similarity between resume and job requirements
9. **Action Verb Identification**: Detect leadership, achievement, and impact verbs
10. **Industry Keyword Matching**: Match against technology, finance, healthcare, marketing, sales domains

**Phase 4: Structural Analysis**
11. **Section Detection**: Identify contact, summary, experience, education, skills, certifications, projects
12. **Format Scoring**: Evaluate resume structure and organization
13. **Completeness Check**: Verify presence of essential sections

**Phase 5: Coding Profile Analysis**
14. **URL Detection**: Regex patterns for LeetCode, CodeForces, GitHub, HackerRank, etc.
15. **API Integration**: Fetch profile data from coding platforms
16. **Rating Analysis**: Evaluate problem-solving skills, contest participation, consistency
17. **Job Readiness Scoring**: Calculate overall coding competency

**Phase 6: LLM Enhancement**
18. **Groq Llama 3.3 Integration**: Send analysis context to LLM
19. **Contextual Understanding**: AI interprets resume quality beyond keywords
20. **Feedback Generation**: Create personalized, actionable improvement suggestions
21. **Strength Identification**: Highlight strong points in the resume

**Phase 7: Score Synthesis**
22. **Category Scores**: Generate scores for keywords, formatting, experience, skills, education, summary
23. **Overall Match Score**: Weighted average of all categories (0-100)
24. **Score Level Classification**: Poor/Fair/Good/Excellent/Outstanding
25. **Missing Keywords**: Identify important terms absent from resume
26. **Recommendations**: Prioritized list of improvements

**Output**: Comprehensive JSON response with scores, feedback, metrics, and actionable insights

### 3. Resume Generation Service (`/ResumeGen/resume_generator.py`)
**Technology**: Flask + Ollama
**Port**: 4000

**Generation Pipeline:**
1. **User Input Collection**: Name, email, phone, experience, education, skills, achievements
2. **AI Content Enhancement** (Ollama Mistral):
   - Professional summary generation
   - Achievement bullet point refinement
3. **PDF Generation** (FPDF):
   - Modern Template (Blue accent color)
   - Dark Theme Template (Black background, white text)
   - Minimalist Template (Clean B&W design)
4. **Batch Export**: ZIP file containing all 3 PDFs

### 4. Main Frontend (`/frontend`)
**Technology**: React + Vite + TailwindCSS

**Pages & Features:**
- Landing page with animations (GSAP)
- User authentication (login/signup with OAuth support)
- Dashboard with portfolio overview
- Portfolio editor (projects, experience, skills, certificates)
- Profile management
- Resume upload & management
- ATS analysis interface with score visualization

---

## 🔄 Pipeline & Workflow

### User Authentication Flow
```
1. User visits frontend → Clicks "Sign Up/Login"
2. Frontend sends request → Backend /api/auth/login or /api/auth/signup
3. Local Strategy: Email/password validated → bcrypt comparison
   OR
   OAuth Strategy: Redirect to Google/GitHub → Callback to backend
4. Backend generates JWT → Signs token with JWT_SECRET
5. Token sent to frontend → Stored in localStorage/sessionStorage
6. Subsequent requests → JWT in Authorization header
7. Backend middleware verifies JWT → Attaches user to req.user
```

### Portfolio Management Flow
```
1. User navigates to portfolio editor
2. Frontend form submission → POST /api/projects (or /experiences, /skills, etc.)
3. Backend auth middleware checks JWT → Validates user
4. Multer middleware handles file upload (if images)
5. Cloudinary upload → Returns secure URL
6. Controller saves to MongoDB → Returns success response
7. Frontend updates UI with new portfolio item
```

### ATS Analysis Flow (Complete Pipeline)
```
┌──────────────┐
│  User Action │
│ Upload Resume│
│  + Job Desc  │
└──────┬───────┘
       │
       ▼
┌─────────────────────────────────────────┐
│  Frontend (POSTMID_ATS/ats-frontend)    │
│  1. File validation (PDF/DOCX)          │
│  2. FormData construction               │
│  3. POST /api/ats/analyze               │
└──────┬──────────────────────────────────┘
       │
       ▼
┌─────────────────────────────────────────┐
│  Backend API (/backend)                 │
│  ┌───────────────────────────────────┐  │
│  │ STEP 1: Auth Middleware           │  │
│  │ - Verify JWT token                │  │
│  │ - Extract user ID                 │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ STEP 2: File Upload (Multer)     │  │
│  │ - Validate file type              │  │
│  │ - Size limit check (5MB)          │  │
│  │ - Buffer resume to memory         │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ STEP 3: Malware Scanning          │  │
│  │ - Scan buffer for threats         │  │
│  │ - Reject if infected              │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ STEP 4: Resume Parsing            │  │
│  │ - Extract text (PDF/DOCX)         │  │
│  │ - Clean and normalize             │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ STEP 5: PII Redaction             │  │
│  │ - Pseudonymize emails             │  │
│  │ - Mask phone numbers              │  │
│  │ - Redact SSN, addresses           │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ STEP 6: Forward to Python Service│  │
│  │ - HTTP POST to ats3.py            │  │
│  │ - Send redacted text + job desc   │  │
│  └───────────┬───────────────────────┘  │
└──────────────┼───────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│  Python ATS Service (ats3.py)           │
│  ┌───────────────────────────────────┐  │
│  │ NLP PROCESSING                    │  │
│  │ - Tokenization (NLTK)             │  │
│  │ - NER (spaCy)                     │  │
│  │ - TF-IDF vectorization            │  │
│  │ - Cosine similarity calculation   │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ KEYWORD ANALYSIS                  │  │
│  │ - Extract resume keywords         │  │
│  │ - Extract job desc keywords       │  │
│  │ - Calculate overlap               │  │
│  │ - Identify missing keywords       │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ CODING PROFILE DETECTION          │  │
│  │ - Regex URL extraction            │  │
│  │ - API calls to coding platforms   │  │
│  │ - Rating and activity analysis    │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ LLM ENHANCEMENT (Groq Llama 3.3)  │  │
│  │ - Contextual understanding        │  │
│  │ - Generate detailed feedback      │  │
│  │ - Create improvement suggestions  │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ SCORE GENERATION                  │  │
│  │ - Overall match score (0-100)     │  │
│  │ - Category scores (6 categories)  │  │
│  │ - Score level & color             │  │
│  │ - Metrics compilation             │  │
│  └───────────┬───────────────────────┘  │
│              │                           │
│  ┌───────────▼───────────────────────┐  │
│  │ Return JSON Response:             │  │
│  │ {                                 │  │
│  │   overall_score: 85,              │  │
│  │   score_level: "Excellent",       │  │
│  │   detailed_scores: {...},         │  │
│  │   feedback: {...},                │  │
│  │   metrics: {...}                  │  │
│  │ }                                 │  │
│  └───────────┬───────────────────────┘  │
└──────────────┼───────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│  Backend API (Continue)                 │
│  ┌───────────────────────────────────┐  │
│  │ STEP 7: Save Analysis Report      │  │
│  │ - Create AtsAnalysis document     │  │
│  │ - Store in MongoDB                │  │
│  │ - Link to user ID                 │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ STEP 8: Store Score History       │  │
│  │ - Check if score changed (>1%)    │  │
│  │ - Create AtsHistoryScore entry    │  │
│  │ - Enable trend tracking           │  │
│  └───────────┬───────────────────────┘  │
│              ▼                           │
│  ┌───────────────────────────────────┐  │
│  │ STEP 9: Return Response           │  │
│  │ - Analysis ID                     │  │
│  │ - Summary (score + keyword count) │  │
│  │ - Success message                 │  │
│  └───────────┬───────────────────────┘  │
└──────────────┼───────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────┐
│  Frontend (Display Results)             │
│  1. Receive analysis ID                 │
│  2. Fetch full report via:              │
│     GET /api/ats/analysis/:analysisId   │
│  3. Render score charts (Chart.js)      │
│  4. Display feedback sections           │
│  5. Show improvement suggestions        │
│  6. Enable export/download              │
└─────────────────────────────────────────┘
```

### Resume Generation Flow
```
1. User fills resume form (name, experience, skills, etc.)
2. Frontend → POST /generate-resume to ResumeGen service
3. Flask receives JSON data
4. Ollama Mistral generates:
   - Professional summary
   - Enhanced achievement bullet points
5. FPDF creates 3 PDFs (Modern, Dark, Minimalist)
6. PDFs saved to /output directory
7. Creates ZIP file with all 3 resumes
8. Returns ZIP as downloadable attachment
9. Frontend triggers download
```

---

## 📁 Project Structure

```
Pr_Project/
│
├── backend/                          # Node.js/Express Backend
│   ├── api/
│   │   └── index.js                  # Vercel serverless entry point
│   ├── config/
│   │   ├── passport.js               # Passport.js strategies (JWT, Google, GitHub)
│   │   ├── cloudinary.js             # Cloudinary configs (multiple)
│   │   └── ...
│   ├── controller/
│   │   ├── authController.js         # Authentication logic
│   │   ├── atsController.js          # ATS analysis orchestration
│   │   ├── portfolioDetailsController.js
│   │   ├── projectController.js
│   │   ├── experienceController.js
│   │   └── ...
│   ├── middleware/
│   │   ├── auth.js                   # JWT verification middleware
│   │   ├── errorHandler.js           # Centralized error handling
│   │   ├── upload.js                 # Multer file upload (multiple configs)
│   │   └── ...
│   ├── models/
│   │   ├── User.js                   # User schema with OAuth support
│   │   ├── AtsAnalysis.js            # ATS report storage
│   │   ├── AtsHistoryScore.js        # Score tracking over time
│   │   ├── PortfolioDetails.js       # Portfolio metadata
│   │   ├── Project.js, Experience.js, Skill.js, Certificate.js
│   │   └── ...
│   ├── routes/
│   │   ├── auth.js                   # /api/auth/* routes
│   │   ├── ats.js                    # /api/ats/* routes
│   │   ├── resume.js                 # /api/resumes/* routes
│   │   ├── portfolioDetails.js       # /api/portfolio/* routes
│   │   └── ...
│   ├── utils/
│   │   ├── logger.js                 # Winston/console logger
│   │   ├── malwareScanner.js         # File scanning utility
│   │   ├── piiRedactor.js            # PII pseudonymization
│   │   ├── resumeParser.js           # PDF/DOCX text extraction
│   │   └── encryption.js             # Data encryption utilities
│   ├── db/
│   │   └── mongoClient.js            # MongoDB connection with caching
│   ├── server.js                     # Main Express app
│   ├── package.json
│   ├── vercel.json                   # Vercel deployment config
│   └── .env                          # Environment variables
│
├── frontend/                         # React Frontend
│   ├── src/
│   │   ├── assets/                   # Images, fonts, static files
│   │   ├── components/               # Reusable React components
│   │   │   ├── Auth/                 # Login, Signup components
│   │   │   ├── Portfolio/            # Portfolio editor components
│   │   │   ├── ATS/                  # ATS analysis UI components
│   │   │   └── Common/               # Shared UI components
│   │   ├── pages/                    # Page-level components
│   │   │   ├── Home.jsx              # Landing page
│   │   │   ├── Dashboard.jsx         # User dashboard
│   │   │   ├── Profile.jsx           # User profile
│   │   │   └── ATSAnalysis.jsx       # ATS analysis page
│   │   ├── styles/                   # CSS modules & Tailwind
│   │   ├── utils/                    # Helper functions
│   │   ├── App.jsx                   # Root component
│   │   └── main.jsx                  # Entry point
│   ├── public/                       # Static public assets
│   ├── package.json
│   ├── vite.config.js                # Vite configuration
│   ├── tailwind.config.js            # Tailwind CSS config
│   └── vercel.json                   # Vercel SPA routing config
│
├── ats3.py                           # FastAPI ATS Service
│                                     # Main production ATS analysis service
│                                     # - Comprehensive hybrid AI+NLP pipeline
│                                     # - Groq Llama 3.3 integration
│                                     # - Coding profile detection & analysis
│                                     # - spaCy, NLTK, sklearn ML processing
│
├── ResumeGen/
│   ├── resume_generator.py           # Flask Resume Generation Service
│   │                                 # - Ollama Mistral integration
│   │                                 # - 3 PDF themes (Modern, Dark, Minimalist)
│   │                                 # - ZIP export
│   ├── output/                       # Generated resume storage
│   └── requirements.txt              # Python dependencies
│
├── README.md                         # This file
├── package.json                      # Root dependencies
└── .gitignore
```

---

## 🚀 Installation & Setup

### Prerequisites
- Node.js >= 18.x
- Python >= 3.9
- MongoDB (local or Atlas cluster)
- Cloudinary account
- Groq API key
- Ollama installed locally (for resume generation)

### 1. Clone Repository
```bash
git clone https://github.com/your-username/Trackfolio.git
cd Trackfolio
```

### 2. Backend Setup
```bash
cd backend
npm install

# Create .env file
cat > .env << EOF
NODE_ENV=development
PORT=5000
MONGODB_URI=mongodb://localhost:27017/Trackfolio
# OR for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/Trackfolio

JWT_SECRET=your_super_secret_jwt_key_change_me
SESSION_SECRET=your_session_secret_change_me

# OAuth Credentials
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
GITHUB_CLIENT_ID=your_github_client_id
GITHUB_CLIENT_SECRET=your_github_client_secret

# URLs
BACKEND_URL=http://localhost:5000
CLIENT_URL=http://localhost:5173

# Cloudinary
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
EOF

# Start backend
npm run dev
# Backend runs on http://localhost:5000
```

### 3. Frontend Setup
```bash
cd ../frontend
npm install

# Create .env file
cat > .env << EOF
VITE_API_URL=http://localhost:5000
VITE_ATS_API_URL=http://localhost:8000
EOF

npm run dev
# Frontend runs on http://localhost:5173
```

### 4. Python Services Setup

#### ATS Analysis Service (ats3.py)
```bash
cd ../../
pip install -r requirements.txt  # If requirements.txt exists
# OR install manually:
pip install fastapi uvicorn python-multipart PyPDF2 python-docx \
            spacy nltk scikit-learn numpy pydantic groq aiohttp \
            beautifulsoup4 PyMuPDF

# Download spaCy model
python -m spacy download en_core_web_sm

# Create .env or set environment variable
export GROQ_API_KEY=your_groq_api_key_here

# Run service
uvicorn ats3:app --reload --port 8000
# Service runs on http://localhost:8000
```

#### Resume Generation Service
```bash
cd ResumeGen

# Install dependencies
pip install flask flask-cors fpdf

# Ensure Ollama is installed and running
ollama pull mistral

# Run Flask service
python resume_generator.py
# Service runs on http://localhost:4000
```

### 5. Verify Installation
- Backend API: http://localhost:5000/api/health
- Frontend: http://localhost:5173
- ATS API (ats3.py): http://localhost:8000/docs (FastAPI interactive docs)
- Resume Gen API: http://localhost:4000

---

## 🌐 Deployment

### Backend (Vercel)
```bash
cd backend
# Ensure api/index.js and vercel.json are present
vercel --prod

# Set environment variables in Vercel dashboard:
# - All variables from .env
# - NODE_ENV=production
# - ENABLE_SERVER_LISTEN=false (disable app.listen in serverless)
```

### Frontend (Vercel)
```bash
cd frontend
vercel --prod

# Ensure vercel.json has SPA routing config
# Update VITE_API_URL to backend production URL
```

### Python Services (Railway)
```bash
# Railway supports Dockerfile or automatic Python detection
# For ats3.py:
railway up
# Set GROQ_API_KEY in Railway dashboard

# For ResumeGen:
railway up
# Ensure Ollama is available or use cloud LLM alternative
```

---

## 📚 API Documentation

### Authentication Endpoints

#### `POST /api/auth/signup`
Register a new user with email/password.

**Request Body:**
```json
{
  "username": "johndoe",
  "displayName": "John Doe",
  "email": "john@example.com",
  "password": "SecurePass123!"
}
```

**Response:**
```json
{
  "success": true,
  "message": "User registered successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "_id": "60d5f...",
    "username": "johndoe",
    "email": "john@example.com"
  }
}
```

#### `POST /api/auth/login`
Login with email/password.

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "SecurePass123!"
}
```

#### `GET /api/auth/google`
Initiate Google OAuth login (redirects to Google).

#### `GET /api/auth/google/callback`
Google OAuth callback (handled by Passport).

#### `GET /api/auth/github`
Initiate GitHub OAuth login.

#### `GET /api/auth/github/callback`
GitHub OAuth callback.

#### `POST /api/auth/logout`
Logout user (destroy session).

---

### ATS Analysis Endpoints

#### `POST /api/ats/analyze`
Upload resume and job description for ATS analysis.

**Headers:**
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: multipart/form-data
```

**Form Data:**
- `resumeFile`: File (PDF/DOCX, max 5MB)
- `resumeTitle`: String (optional)
- `jobDescription`: String (optional, min 50 chars)

**Response:**
```json
{
  "success": true,
  "message": "Resume analyzed successfully",
  "analysisId": "60d5f...",
  "summary": {
    "matchScore": 85,
    "keywordCount": 12
  }
}
```

#### `GET /api/ats/analysis/:analysisId`
Retrieve a specific ATS analysis report.

**Response:**
```json
{
  "success": true,
  "analysis": {
    "_id": "60d5f...",
    "resumeTitle": "John_Doe_Resume.pdf",
    "jobTitle": "Senior Software Engineer",
    "analysisDate": "2026-03-30T10:30:00Z",
    "summary": { "matchScore": 85, "keywordCount": 12 },
    "reportData": "{...full analysis JSON...}"
  }
}
```

#### `GET /api/ats/history`
Get all ATS analyses for authenticated user.

#### `DELETE /api/ats/analysis/:analysisId`
Delete a specific ATS analysis.

#### `GET /api/ats/score-history`
Get ATS score history for chart visualization.

**Query Parameters:**
- `period`: "7d", "30d", "90d", "1y", "all" (default: "all")
- `limit`: Number (default: 50)

**Response:**
```json
{
  "success": true,
  "data": {
    "history": [...],
    "statistics": {
      "totalEntries": 15,
      "averageScore": 78,
      "highestScore": 92,
      "lowestScore": 65,
      "improvement": 10
    }
  }
}
```

#### `POST /api/ats/keywords`
Extract keywords from text (e.g., job description).

**Request Body:**
```json
{
  "text": "We are seeking a Python developer with 5+ years experience..."
}
```

**Response:**
```json
{
  "success": true,
  "keywords": ["python", "developer", "experience", "flask", "django", "api", ...]
}
```

---

### Portfolio Endpoints

#### `POST /api/projects`
Create a new project.

**Headers:**
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: multipart/form-data
```

**Form Data:**
- `projectName`: String
- `description`: String
- `technologies`: String (comma-separated)
- `projectImage`: File (optional)
- `projectUrl`: String (optional)
- `githubRepo`: String (optional)

#### `GET /api/projects`
Get all projects for authenticated user.

#### `PUT /api/projects/:id`
Update a project.

#### `DELETE /api/projects/:id`
Delete a project.

Similar endpoints exist for:
- `/api/experiences`
- `/api/skills`
- `/api/certificates`
- `/api/portfolio` (portfolio details/metadata)

---

### Resume Endpoints

#### `POST /api/resumes/`
Upload resume file to MongoDB/Cloudinary.

#### `GET /api/resumes/`
Get all resumes for authenticated user.

#### `GET /api/resumes/:id`
Get specific resume metadata.

#### `GET /api/resumes/pdf/:id`
Get resume PDF file.

#### `DELETE /api/resumes/:id`
Delete a resume.

---

### Python Service Endpoints

#### ATS Analysis Service (ats3.py)

**`POST http://localhost:8000/analyze`**

Upload resume file for comprehensive ATS analysis.

**Request Body (multipart/form-data):**
- `resumeFile`: File (PDF/DOCX)
- `jobDescription`: String (optional)

**OR (JSON):**
```json
{
  "resume_text": "Senior Software Engineer with 5 years...",
  "job_description": "We are seeking a Python developer..."
}
```

**Response:**
```json
{
  "overall_score": 85.5,
  "score_level": "Excellent",
  "score_color": "#10b981",
  "detailed_scores": {
    "keywords": 82,
    "formatting": 90,
    "experience": 85,
    "skills": 88,
    "education": 86,
    "summary": 80
  },
  "feedback": {
    "strengths": [
      "Strong keyword match with job requirements",
      "Well-structured resume with clear sections",
      "Relevant technical skills highlighted"
    ],
    "improvements": [
      "Add more quantifiable achievements",
      "Include specific project outcomes",
      "Expand on leadership experience"
    ]
  },
  "missing_keywords": ["Docker", "AWS", "CI/CD"],
  "metrics": {
    "word_count": 450,
    "keyword_density": 12.5,
    "action_verb_count": 18
  },
  "coding_profiles": [
    {
      "platform": "leetcode",
      "username": "johndoe",
      "rating": 1850,
      "problems_solved": 250,
      "consistency_score": 0.85
    }
  ],
  "analysis_timestamp": "2026-03-30T10:30:00Z"
}
```

#### Resume Generation (ResumeGen)

**`POST http://localhost:4000/generate-resume`**

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+1-555-1234",
  "experience": [...],
  "education": [...],
  "skills": ["Python", "React", "AWS"],
  "achievements": ["Increased revenue by 30%"]
}
```

**Response:**
ZIP file download with 3 PDFs:
- `modern_resume_20260330_103000.pdf`
- `dark_resume_20260330_103000.pdf`
- `minimalist_resume_20260330_103000.pdf`

---

## 🔒 Security Features

### 1. Authentication Security
- **JWT Tokens**: Signed with HS256, 24-hour expiration
- **Password Hashing**: bcryptjs with 10 salt rounds
- **OAuth2.0**: Google and GitHub authentication
- **Session Management**: MongoDB-backed sessions with httpOnly cookies

### 2. Data Protection
- **PII Redaction**: Automatic pseudonymization of:
  - Email addresses → `[EMAIL_REDACTED_abc123]`
  - Phone numbers → `[PHONE_REDACTED_xyz789]`
  - SSN, addresses, credit cards
- **Encryption**: Sensitive data encrypted at rest (encryption.js utility)
- **Malware Scanning**: Resume files scanned before processing

### 3. Network Security
- **CORS**: Strict origin control from environment variables
- **Helmet.js**: Security headers (XSS protection, CSP, HSTS)
- **Rate Limiting**: 100 requests per 15 minutes on auth endpoints
- **Input Validation**: Joi schemas on all user inputs

### 4. File Upload Security
- **Type Restriction**: Only PDF/DOCX for resumes, JPG/PNG for images
- **Size Limits**: 5MB for resumes, varies for other files
- **Sanitization**: Filenames sanitized, suspicious files rejected

### 5. Logging & Monitoring
- **Structured Logging**: Winston-based logger with log levels
- **Security Events**: Special `logger.security()` for security-critical events
- **Error Tracking**: Centralized error handler with stack traces

---

## 👥 Team Structure

| Name | Role | Responsibilities |
|------|------|-----------------|
| **Salugu Harshita Bhanu** | Frontend & Security Lead | UI/UX implementation, React components, state management, animations, authentication systems, security protocols, PII protection, OAuth implementation |
| **Divanshu Bhargava** | ML Lead | ATS algorithm development, NLP integration, LLM integration, coding profile analysis |
| **Aryan Kesarwani** | Fullstack Developer | Backend API development, database design, microservices architecture |
| **Aayushi Thakre** | ML Lead | Recommendation systems, data modeling, AI-powered features |

---





---

## 👨‍💻 Contributors

This project is a collaborative effort by a talented team of developers:

- **Harshita Bhanu** - Frontend & Security Lead
  GitHub: [@Git-brintsi20](https://github.com/Git-brintsi20)

- **Aayushi Thakre** - AI/ML Engineering & LLM Integration
  GitHub: [@meoyushi](https://github.com/meoyushi)

- **Aryan Kesarwani** - Backend Architecture & API Development
  GitHub: [@Speedy2705](https://github.com/Speedy2705)

- **Divanshu Bhargava** - AI/ML Engineering, NLP & Research
  GitHub: [@divanshu0212](https://github.com/divanshu0212/)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Groq** for providing fast LLM inference
- **Ollama** for local LLM deployment
- **MongoDB Atlas** for cloud database hosting
- **Vercel** for seamless frontend/backend deployment
- **Railway** for Python service hosting
- **Cloudinary** for asset management
- **OpenAI/Anthropic** for inspiring AI-powered workflows

---

## 📞 Support & Contact

- **Issues**: [GitHub Issues](https://github.com/divanshu0212/Trackfolio/issues)
- **Discussions**: [GitHub Discussions](https://github.com/divanshu0212/Trackfolio/discussions)

---

<div align="center">

### ⭐ Star this repository if you find it helpful!

**Built with ❤️ by Team Trackfolio | Capstone Project 2025**

</div>
