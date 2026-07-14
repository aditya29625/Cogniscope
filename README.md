<div align="center">

# 🧠 Cogniscope

### AI-Powered Cognitive Health Assessment Platform

*Early detection and prevention of dementia through adaptive AI-generated assessments*

[![Next.js](https://img.shields.io/badge/Next.js-16.0-black?style=for-the-badge&logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-3178C6?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org)
[![Prisma](https://img.shields.io/badge/Prisma-6.19-2D3748?style=for-the-badge&logo=prisma)](https://prisma.io)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=for-the-badge&logo=postgresql)](https://postgresql.org)
[![Gemini AI](https://img.shields.io/badge/Gemini-AI-4285F4?style=for-the-badge&logo=google)](https://ai.google.dev)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [System Architecture](#-system-architecture)
- [Tech Stack](#-tech-stack)
- [Database Schema](#-database-schema)
- [User Flow](#-user-flow)
- [API Routes](#-api-routes)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Project Structure](#-project-structure)
- [Cognitive Categories](#-cognitive-categories)

---

## 🌟 Overview

**Cogniscope** is a full-stack web application designed to help detect early signs of cognitive decline through AI-powered assessments. It leverages **Google Gemini AI** to dynamically generate personalized cognitive tests and produce detailed clinical-grade reports.

> 🏥 Designed with a medical-tech aesthetic — black, white, and red — to convey precision, trust, and urgency.

---

## ✨ Features

| Feature | Description |
|--------|-------------|
| 🤖 **AI Question Generation** | Gemini AI generates unique questions every session |
| 📝 **MCQ Test Mode** | 20 adaptive multiple-choice cognitive questions |
| 🎙️ **Voice Test Mode** | Speech-to-text powered voice response assessment |
| 📊 **Detailed Reports** | AI-analyzed reports with risk level, cognitive age & recommendations |
| 🔐 **Secure Auth** | NextAuth.js with bcrypt-hashed passwords |
| 📈 **Dashboard** | Personal stats, test history, and progress tracking |
| 🏥 **Doctors List** | Connect with healthcare professionals |
| 📱 **Fully Responsive** | Works on desktop, tablet, and mobile |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        COGNISCOPE SYSTEM                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌──────────────┐     HTTPS      ┌──────────────────────────┐ │
│   │   Browser    │◄──────────────►│   Next.js 16 App Router  │ │
│   │  (React 19)  │                │   (Server + Client Side)  │ │
│   └──────────────┘                └────────────┬─────────────┘ │
│                                                │               │
│                          ┌─────────────────────┼──────────┐    │
│                          │                     │          │    │
│                    ┌─────▼──────┐    ┌─────────▼──────┐   │    │
│                    │  NextAuth  │    │   API Routes    │   │    │
│                    │  (Session  │    │  /api/auth/**   │   │    │
│                    │  Manager)  │    │  /api/test/**   │   │    │
│                    └─────┬──────┘    │  /api/report/** │   │    │
│                          │          └────────┬─────────┘   │    │
│                          │                  │              │    │
│                   ┌──────▼──────────────────▼────────┐    │    │
│                   │         Prisma ORM Layer          │    │    │
│                   └──────────────┬────────────────────┘    │    │
│                                  │                         │    │
│                   ┌──────────────▼───────────┐            │    │
│                   │   PostgreSQL Database     │            │    │
│                   │  users | test_sessions    │            │    │
│                   │  questions | answers      │            │    │
│                   │  reports | cognitive_scores│           │    │
│                   └──────────────────────────┘            │    │
│                                                            │    │
│                   ┌────────────────────────────────────┐  │    │
│                   │          Google Gemini AI           │  │    │
│                   │  • Question generation              │  │    │
│                   │  • Report analysis                  │  │    │
│                   │  • Voice transcription              │  │    │
│                   └────────────────────────────────────┘  │    │
│                                                            │    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

```
Frontend
├── Next.js 16       (App Router, Server Components)
├── React 19         (UI Library)
├── TypeScript 5     (Type Safety)
├── Tailwind CSS 4   (Styling)
├── Framer Motion    (Animations)
└── Lucide React     (Icons)

Backend
├── Next.js API Routes  (REST Endpoints)
├── NextAuth.js 4       (Authentication)
├── Prisma 6            (ORM)
├── bcryptjs            (Password Hashing)
└── Zod                 (Schema Validation)

Database
└── PostgreSQL 15       (Relational Database)

AI / ML
└── Google Gemini AI    (@google/generative-ai)
    ├── gemini-1.5-pro  (Question Generation & Reporting)
    └── Speech-to-Text  (Voice Test Transcription)
```

---

## 🗄️ Database Schema

```
┌──────────────────┐       ┌──────────────────────┐
│      users       │       │    test_sessions      │
├──────────────────┤       ├──────────────────────┤
│ id (PK)          │──────►│ id (PK)              │
│ email (UNIQUE)   │  1:N  │ userId (FK)          │
│ name             │       │ testType (MCQ/VOICE) │
│ password (hash)  │       │ status               │
│ createdAt        │       │ startedAt            │
│ updatedAt        │       │ completedAt          │
└──────────────────┘       │ totalQuestions       │
                           │ answeredCount        │
                           │ durationSeconds      │
                           └──────────┬───────────┘
                                      │
                  ┌───────────────────┼────────────────────┐
                  │                   │                    │
        ┌─────────▼───────┐  ┌────────▼──────┐  ┌─────────▼──────┐
        │    questions    │  │    answers    │  │    reports     │
        ├─────────────────┤  ├───────────────┤  ├────────────────┤
        │ id (PK)         │  │ id (PK)       │  │ id (PK)        │
        │ testSessionId   │  │ testSessionId │  │ userId (FK)    │
        │ questionType    │  │ questionId    │  │ testSessionId  │
        │ questionText    │  │ userAnswer    │  │ overallScore   │
        │ options[]       │  │ isCorrect     │  │ riskLevel      │
        │ correctAnswer   │  │ responseTime  │  │ cognitiveAge   │
        │ category        │  │ confidence    │  │ memoryScore    │
        │ difficulty      │  │ audioData     │  │ attentionScore │
        │ orderIndex      │  │ transcribed   │  │ summary (AI)   │
        └─────────────────┘  └───────────────┘  │ recommendations│
                                                 └────────────────┘
                                ┌──────────────────────┐
                                │   cognitive_scores   │
                                ├──────────────────────┤
                                │ id (PK)              │
                                │ testSessionId (FK)   │
                                │ category             │
                                │ score / maxScore     │
                                │ percentile           │
                                │ accuracy             │
                                │ avgResponseTime      │
                                └──────────────────────┘
```

---

## 🔄 User Flow

```
                    ┌─────────────┐
                    │   Landing   │
                    │    Page     │
                    └──────┬──────┘
                           │
              ┌────────────┴────────────┐
              │                         │
       ┌──────▼──────┐           ┌──────▼──────┐
       │   Sign Up   │           │    Login    │
       └──────┬──────┘           └──────┬──────┘
              └────────────┬────────────┘
                           │
                    ┌──────▼──────┐
                    │  Dashboard  │◄──────────────────┐
                    │  (Stats &   │                   │
                    │  History)   │                   │
                    └──────┬──────┘                   │
                           │                          │
              ┌────────────┴────────────┐             │
              │                         │             │
       ┌──────▼──────┐           ┌──────▼──────┐      │
       │  MCQ Test   │           │ Voice Test  │      │
       │  (20 Qs)    │           │ (20 Qs)     │      │
       └──────┬──────┘           └──────┬──────┘      │
              └────────────┬────────────┘             │
                           │                          │
                    ┌──────▼──────┐                   │
                    │  AI Scores  │                   │
                    │  & Analysis │                   │
                    └──────┬──────┘                   │
                           │                          │
                    ┌──────▼──────┐                   │
                    │   Report    │───────────────────►│
                    │ Generation  │    (save & return) │
                    │ (Gemini AI) │                   │
                    └─────────────┘                   │
```

---

## 🔌 API Routes

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/auth/signup` | Register new user |
| `POST` | `/api/auth/[...nextauth]` | Login / NextAuth handler |
| `POST` | `/api/test/start` | Start a new test session |
| `GET`  | `/api/test/[id]/questions` | Fetch AI-generated questions |
| `POST` | `/api/test/[id]/answer` | Submit an answer |
| `POST` | `/api/test/[id]/complete` | Complete test & trigger report |
| `GET`  | `/api/report/[id]` | Fetch a specific report |
| `GET`  | `/api/dashboard` | Get user stats & history |
| `GET`  | `/api/doctors` | List of doctors |

---

## 🧪 Cognitive Categories Assessed

```
┌─────────────────────────────────────────────────────────┐
│              COGNITIVE DOMAINS EVALUATED                │
├───────────────────────┬─────────────────────────────────┤
│  🧠 Memory Recall     │ Short & long-term memory tests  │
│  🎯 Attention         │ Focus and concentration tasks   │
│  ⚡ Processing Speed  │ Response time measurement       │
│  🔧 Executive Func.  │ Planning & problem solving       │
│  💬 Language          │ Comprehension & verbal fluency  │
│  🗺️ Visual-Spatial   │ Pattern recognition & spatial   │
└───────────────────────┴─────────────────────────────────┘

Risk Levels:
  🟢 LOW      — No significant concerns
  🟡 MODERATE — Early markers detected, monitoring advised  
  🟠 HIGH     — Clinical evaluation recommended
  🔴 CRITICAL — Immediate professional consultation needed
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** v18+
- **PostgreSQL** v14+
- **Google Gemini API Key** → [Get one free](https://aistudio.google.com/app/apikey)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/aditya29625/Cogniscope.git
cd Cogniscope

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env
# Edit .env with your values (see below)

# 4. Run database migrations
npx prisma migrate dev --name init

# 5. Start the development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🔑 Environment Variables

Create a `.env` file in the root directory:

```env
# Database (PostgreSQL)
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/cogniscope"

# NextAuth
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-random-secret-here"

# Google Gemini AI
GEMINI_API_KEY="your-gemini-api-key-here"
```

| Variable | Where to get it |
|----------|----------------|
| `DATABASE_URL` | [Neon.tech](https://neon.tech) (free) or local PostgreSQL |
| `NEXTAUTH_SECRET` | Run: `openssl rand -base64 32` |
| `GEMINI_API_KEY` | [Google AI Studio](https://aistudio.google.com/app/apikey) (free) |

---

## 📁 Project Structure

```
cogniscope/
├── prisma/
│   ├── schema.prisma          # Database models
│   └── migrations/            # SQL migration files
├── src/
│   ├── app/
│   │   ├── api/               # API route handlers
│   │   │   ├── auth/          # Authentication endpoints
│   │   │   ├── test/          # Test session management
│   │   │   ├── report/        # Report generation
│   │   │   ├── dashboard/     # User dashboard data
│   │   │   └── doctors/       # Doctors listing
│   │   ├── auth/
│   │   │   ├── login/         # Login page
│   │   │   └── signup/        # Registration page
│   │   ├── dashboard/         # User dashboard
│   │   ├── test/[id]/         # Test taking interface
│   │   ├── reports/[id]/      # Report view
│   │   └── page.tsx           # Landing page
│   ├── components/            # Reusable UI components
│   ├── lib/
│   │   ├── prisma.ts          # Prisma client singleton
│   │   ├── gemini.ts          # Gemini AI integration
│   │   └── auth.ts            # NextAuth configuration
│   └── types/                 # TypeScript type definitions
├── public/                    # Static assets
├── .env.example               # Environment variables template
├── next.config.ts             # Next.js configuration
├── tailwind.config.ts         # Tailwind CSS configuration
├── tsconfig.json              # TypeScript configuration
└── package.json
```

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/AmazingFeature`
3. Commit your changes: `git commit -m 'Add some AmazingFeature'`
4. Push to the branch: `git push origin feature/AmazingFeature`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

Built with ❤️ by **Aditya Dhanraj Singh**

⭐ Star this repo if you found it helpful!

</div>
