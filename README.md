<div align="center">

# 📚 StudyMate

### Smart Notes & Semantic Q&A Bank for Coursework

**A coursework knowledge management and semantic retrieval platform powered by PostgreSQL, pgvector, and modern web technologies.**

*Smart Notes. Semantic Search. Better Preparation.*

</div>

---

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Objectives](#objectives)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [End-to-End Data Flow](#end-to-end-data-flow)
- [Technology Stack](#technology-stack)
- [AI/ML Components](#aiml-components)
- [Database Architecture](#database-architecture)
- [Database Schema](#database-schema)
- [Semantic Search Pipeline](#semantic-search-pipeline)
- [Hybrid Search](#hybrid-search)
- [Q&A Bank](#qa-bank)
- [Quiz and Assessment System](#quiz-and-assessment-system)
- [Learning Analytics](#learning-analytics)
- [Authentication and Authorization](#authentication-and-authorization)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
- [API Overview](#api-overview)
- [Database Search Flow](#database-search-flow)
- [Testing](#testing)
- [Performance Evaluation](#performance-evaluation)
- [Security](#security)
- [Git Workflow](#git-workflow)
- [Development Roadmap](#development-roadmap)
- [Project Documentation](#project-documentation)
- [Future Enhancements](#future-enhancements)
- [Current Implementation Status](#current-implementation-status)
- [Project Goals](#project-goals)
- [Contributors](#contributors)
- [License](#license)
- [Acknowledgements](#acknowledgements)

---

## Overview

Students typically store academic information across multiple disconnected sources such as PDFs, lecture notes, question banks, and personal documents. Traditional keyword-based search is often insufficient when students remember the concept they are looking for but not the exact terminology used in their notes.

**StudyMate** addresses this problem by combining:

- Structured relational data
- Coursework document management
- PostgreSQL Full-Text Search
- Vector embeddings
- pgvector similarity search
- Hybrid retrieval
- Structured Q&A management
- Quiz-based assessment
- Learning analytics

The system is designed around a central PostgreSQL database, with pgvector providing semantic retrieval capabilities for coursework content.

---

## Problem Statement

Academic study material is often fragmented across lecture notes, PDFs, course units, question banks, and personal resources. Students face several problems:

1. Difficulty organizing large amounts of coursework.
2. Difficulty finding relevant information using traditional keyword search.
3. Lack of semantic search across their own study material.
4. Difficulty maintaining a structured question bank.
5. Limited visibility into topic-wise academic performance.
6. Lack of a unified platform connecting notes, questions, practice, and analytics.

StudyMate provides a unified platform for managing coursework and retrieving academically relevant information using both traditional database queries and semantic similarity search.

---

## Objectives

- Build a structured academic knowledge management system.
- Store coursework information using a relational database.
- Support document-based academic content.
- Convert coursework content into vector embeddings.
- Store embeddings using PostgreSQL and pgvector.
- Implement semantic similarity search.
- Implement PostgreSQL Full-Text Search.
- Combine keyword and semantic retrieval through hybrid search.
- Build a structured Q&A bank.
- Provide quiz and assessment functionality.
- Track student attempts and performance.
- Identify weak topics.
- Provide learning analytics.
- Demonstrate advanced database concepts beyond basic CRUD.

---

## Key Features

### 1. User Authentication
- Student registration and login
- Secure password handling
- Session management
- Role-based authorization (Student and Admin roles)

### 2. Course Management

Academic content is organized hierarchically:

```
Course
   └── Unit
        └── Topic
             └── Notes
```

**Example:**

```
Database Management Systems
│
├── Unit 1
│   ├── ER Model
│   ├── Relational Model
│   └── Relational Algebra
│
├── Unit 2
│   ├── Functional Dependencies
│   ├── Normalization
│   └── Normal Forms
│
└── Unit 3
    ├── Transactions
    ├── Concurrency Control
    └── Recovery
```

### 3. Notes Management

Students can:
- Upload coursework notes
- Associate notes with courses, units, and topics
- Store note metadata
- Extract text from PDF documents
- Divide notes into searchable chunks

### 4. Semantic Search

StudyMate converts coursework chunks into embeddings and stores them in PostgreSQL using pgvector. A natural-language query can retrieve conceptually relevant material even when the exact words do not appear in the document.

**Example:**

> Query: *"How does a database prevent two transactions from interfering with each other?"*

The system can retrieve coursework related to:

```
Concurrency Control
Isolation
Locking
Serializability
```

### 5. Full-Text Search

PostgreSQL Full-Text Search is used for traditional keyword-based retrieval, providing strong results for queries containing exact academic terminology.

### 6. Hybrid Search

StudyMate combines:

```
PostgreSQL Full-Text Search
            +
   pgvector Semantic Search
            ↓
       Hybrid Ranking
            ↓
       Final Results
```

This allows the system to handle both exact terminology and conceptual queries.

### 7. Q&A Bank

Questions can be stored and categorized by Course, Unit, Topic, Difficulty, Question Type, and Tags.

Supported question types include:
- MCQ
- True/False
- Short Answer
- Long Answer
- Fill in the Blank

### 8. Quiz System

Students can configure quizzes using parameters such as Course, Unit, Topic, Difficulty, and Number of Questions. The system selects appropriate questions from the Q&A bank and records the student's attempts.

### 9. Performance Analytics

StudyMate tracks:
- Questions attempted
- Correct / incorrect answers
- Accuracy
- Topic performance
- Course performance
- Weak topics
- Attempt history

**Example:**

| Topic         | Accuracy |
|---------------|----------|
| Normalization | 87%      |
| Transactions  | 61%      |
| Deadlocks     | 48%      |
| Indexing      | 76%      |

### 10. Weak Topic Identification

Student performance is analyzed at the topic level:

```
Attempts → Topic-wise aggregation → Accuracy calculation
         → Compare with threshold → Identify weak topics
         → Retrieve relevant study material
```

This creates a connection between the relational analytics layer and the semantic retrieval layer.

---

## System Architecture

```
                         ┌─────────────────────┐
                         │       STUDENT        │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       Next.js        │
                         │                       │
                         │ TypeScript            │
                         │ Tailwind CSS          │
                         │ shadcn/ui             │
                         │ Auth.js               │
                         └──────────┬───────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │                                │
                    ▼                                ▼
          ┌───────────────────┐           ┌───────────────────┐
          │ Next.js API        │           │ Python FastAPI     │
          │                     │           │                     │
          │ Authentication      │           │ PDF Processing      │
          │ Courses             │           │ Chunking            │
          │ Notes               │           │ Embeddings          │
          │ Questions           │           │ Semantic Search     │
          │ Quizzes             │           │                     │
          │ Analytics           │           │                     │
          └─────────┬───────────┘           └─────────┬───────────┘
                    │                                 │
                    └───────────────┬─────────────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      PostgreSQL       │
                         │                       │
                         │ Relational Data       │
                         │  Users                │
                         │  Courses              │
                         │  Units                │
                         │  Topics               │
                         │  Notes                │
                         │  Questions            │
                         │  Quizzes              │
                         │  Attempts             │
                         │                       │
                         │ Full-Text Search       │
                         │ Views / Functions      │
                         │ Triggers / Transactions│
                         │                       │
                         │ ┌───────────────────┐ │
                         │ │     pgvector        │ │
                         │ │                     │ │
                         │ │ Embeddings          │ │
                         │ │ Vector Search       │ │
                         │ │ Vector Indexes      │ │
                         │ └───────────────────┘ │
                         └─────────────────────┘
```

---

## End-to-End Data Flow

### Coursework Ingestion

```
PDF → Next.js Upload → FastAPI → PyMuPDF
    → Text Extraction → Text Cleaning
    → Semantic Chunking → Embedding Generation
    → Vector Embeddings → PostgreSQL + pgvector
```

### Search Flow

```
User Query
    │
    ▼
Query Processing
    │
    ├───────────────────────┐
    ▼                       ▼
Full-Text Search       Query Embedding
    │                       │
    ▼                       ▼
Keyword Results        pgvector Search
    │                       │
    └───────────┬───────────┘
                ▼
         Hybrid Ranking
                ▼
          Top-K Results
                ▼
               UI
```

### Quiz Flow

```
Quiz Configuration → Question Filtering → Question Selection
    → Quiz → Student Answers → Transaction
        ├── Store Attempt
        ├── Calculate Score
        └── Update Statistics
    → Analytics
```

---

## Technology Stack

| Layer              | Technology                                        |
|---------------------|---------------------------------------------------|
| Frontend            | Next.js                                            |
| Language            | TypeScript                                         |
| Styling             | Tailwind CSS                                       |
| UI Components       | shadcn/ui                                          |
| Application API     | Next.js Route Handlers                             |
| ML API              | Python + FastAPI                                   |
| Database            | PostgreSQL                                         |
| Vector Extension    | pgvector                                           |
| ORM                 | Prisma                                             |
| Embeddings          | Sentence Transformers                              |
| Embedding Model     | `sentence-transformers/all-MiniLM-L6-v2`           |
| Embedding Dimension | 384                                                |
| PDF Processing      | PyMuPDF                                            |
| Search              | PostgreSQL FTS + pgvector                          |
| Authentication      | Auth.js                                            |
| Validation          | Zod                                                |
| Containerization    | Docker + Docker Compose                            |
| Database Client     | DBeaver                                            |
| API Testing         | Bruno / Postman                                    |
| Version Control     | Git + GitHub                                       |

---

## AI/ML Components

### Embedding Model

StudyMate initially uses:

```
sentence-transformers/all-MiniLM-L6-v2
```

The model converts text into 384-dimensional dense vectors:

```
Text → Sentence Transformer → 384-dimensional vector → pgvector
```

### Why Embeddings?

Traditional keyword search requires matching words. Semantic search instead compares the meaning represented by vectors.

**Example:**

> Query: *"How are simultaneous transactions controlled?"*

can retrieve content discussing:

```
Concurrency Control
Locking
Serializability
Isolation
```

even when the exact query wording does not occur in the notes.

---

## Database Architecture

StudyMate uses a single PostgreSQL database with the pgvector extension.

```
                    PostgreSQL
                         │
          ┌──────────────┴──────────────┐
          │                              │
          ▼                              ▼
   Relational Layer                Vector Layer
          │                              │
          ├── Users                      ├── Note Embeddings
          ├── Courses                    ├── Chunk Embeddings
          ├── Units                      └── Question Embeddings
          ├── Topics
          ├── Notes
          ├── Questions
          ├── Quizzes
          └── Attempts
```

This architecture avoids the need for a separate vector database while keeping structured and semantic data within the same PostgreSQL environment.

---

## Database Schema

The core relational model consists of the following entities:

```
users
courses
enrollments
units
topics
notes
note_chunks
questions
tags
question_tags
quizzes
quiz_questions
quiz_attempts
attempts
```

### Users

```
users
--------------------------------
user_id PK
name
email UNIQUE
password_hash
role
created_at
```

### Courses

```
courses
--------------------------------
course_id PK
course_code UNIQUE
course_name
semester
created_at
```

### Enrollments

```
enrollments
--------------------------------
enrollment_id PK
user_id FK
course_id FK
enrolled_at
```

### Units

```
units
--------------------------------
unit_id PK
course_id FK
unit_number
title
description
```

### Topics

```
topics
--------------------------------
topic_id PK
unit_id FK
title
description
```

### Notes

```
notes
--------------------------------
note_id PK
topic_id FK
uploaded_by FK
title
file_name
file_path
created_at
```

### Note Chunks

```
note_chunks
--------------------------------
chunk_id PK
note_id FK
chunk_text
chunk_index
page_number
embedding VECTOR(384)
created_at
```

### Questions

```
questions
--------------------------------
question_id PK
course_id FK
topic_id FK
question_text
answer
difficulty
question_type
created_by FK
created_at
```

### Attempts

```
attempts
--------------------------------
attempt_id PK
user_id FK
question_id FK
selected_answer
is_correct
attempted_at
```

---

## Semantic Search Pipeline

| Step | Description |
|------|--------------|
| 1. Document Upload | Student uploads a PDF |
| 2. Text Extraction | PDF → PyMuPDF → Raw Text |
| 3. Cleaning | Raw Text → Cleaned Text |
| 4. Chunking | Cleaned Text → Multiple Chunks |
| 5. Embedding | Chunk → Sentence Transformer → Vector |
| 6. Storage | Vector → PostgreSQL pgvector |
| 7. Query | User Query → Embedding |
| 8. Similarity Search | Query Vector → pgvector → Nearest Chunks |
| 9. Metadata Filtering | Restrict results by Course / Unit / Topic / Note |
| 10. Ranking | Most relevant chunks returned to the user |

---

## Hybrid Search

StudyMate combines two retrieval mechanisms.

**Keyword Retrieval** — PostgreSQL Full-Text Search handles exact and lexical matching:

```
Query → PostgreSQL FTS → Keyword Results
```

**Semantic Retrieval** — pgvector handles semantic similarity:

```
Query → Embedding → pgvector → Semantic Results
```

**Combined Retrieval:**

```
Keyword Score + Semantic Score = Hybrid Score → Ranked Results
```

Initial ranking uses:

```
final_score =
    keyword_weight * keyword_score
    +
    semantic_weight * semantic_score
```

The weights will be evaluated experimentally during development.

---

## Q&A Bank

The Q&A system stores structured academic questions. Each question can contain:

```
Question, Answer, Course, Unit, Topic, Difficulty, Question Type, Tags, Creator
```

Questions can also be embedded to support semantic question retrieval.

**Example:**

```
User Query: "Questions about database isolation"
        ↓
Question Embedding
        ↓
pgvector
        ↓
Relevant Q&A
```

---

## Quiz and Assessment System

Students can configure a quiz using Course, Unit, Topic, Difficulty, and Number of Questions.

**Example:**

```
Course: DBMS
Unit: 2
Difficulty: Medium
Questions: 10
```

The quiz submission process uses a database transaction to ensure consistent updates:

```
BEGIN
    Create Quiz Attempt
    Insert Question Attempts
    Calculate Score
    Update Statistics
COMMIT
```

If an operation fails:

```
ROLLBACK
```

---

## Learning Analytics

StudyMate uses SQL aggregation to calculate learning performance.

**Example metrics:**

```
Total Questions
Questions Attempted
Correct Answers
Incorrect Answers
Accuracy
Topic Accuracy
Course Accuracy
```

**SQL concepts demonstrated:** `JOIN`, `GROUP BY`, `HAVING`, `COUNT()`, `SUM()`, `AVG()`, `ORDER BY`, Subqueries

### Weak Topic Detection

The system identifies topics where the student's performance falls below a defined threshold:

```
Attempts → GROUP BY Topic → Calculate Accuracy
         → Compare with Threshold → Weak Topic
```

**Example:** `Deadlocks → 48%`

StudyMate can then retrieve Relevant Notes, Relevant Questions, and Practice Material for the weak topic.

---

## Authentication and Authorization

The application supports two primary roles.

### Student
- View enrolled courses
- View notes
- Search coursework
- Access Q&A
- Take quizzes
- View their performance

### Admin
- Manage users
- Manage courses
- Manage units and topics
- Manage questions
- Manage tags
- View system-level statistics

Authorization is enforced at the API layer.

---

## Project Structure

```
studymate/
│
├── frontend/
│   │
│   ├── app/
│   │   ├── api/
│   │   │   ├── auth/
│   │   │   ├── courses/
│   │   │   ├── units/
│   │   │   ├── topics/
│   │   │   ├── notes/
│   │   │   ├── search/
│   │   │   ├── questions/
│   │   │   ├── quizzes/
│   │   │   ├── attempts/
│   │   │   └── analytics/
│   │   │
│   │   ├── dashboard/
│   │   ├── courses/
│   │   ├── notes/
│   │   ├── search/
│   │   ├── questions/
│   │   ├── quiz/
│   │   ├── analytics/
│   │   ├── admin/
│   │   └── ...
│   │
│   ├── components/
│   ├── lib/
│   ├── prisma/
│   │   └── schema.prisma
│   ├── types/
│   ├── public/
│   ├── package.json
│   └── ...
│
├── ml-service/
│   │
│   ├── app/
│   │   ├── api/
│   │   ├── services/
│   │   │   ├── pdf_service.py
│   │   │   ├── chunking_service.py
│   │   │   ├── embedding_service.py
│   │   │   └── search_service.py
│   │   ├── models/
│   │   └── main.py
│   │
│   ├── requirements.txt
│   └── ...
│
├── database/
│   │
│   ├── migrations/
│   ├── seeds/
│   ├── functions/
│   ├── triggers/
│   ├── views/
│   └── sql/
│
├── docs/
│   ├── er-diagram/
│   ├── architecture/
│   ├── database/
│   └── testing/
│
├── docker/
│
├── docker-compose.yml
├── .env.example
├── .gitignore
└── README.md
```

---

## Prerequisites

Install the following:

- Node.js
- npm
- Python 3.x
- Docker Desktop
- Git
- VS Code
- DBeaver or another PostgreSQL client

Verify installations:

```bash
node --version
npm --version
python --version
docker --version
git --version
```

---

## Installation

**1. Clone the repository**

```bash
git clone <repository-url>
cd studymate
```

**2. Start PostgreSQL + pgvector**

```bash
docker compose up -d
```

Verify:

```bash
docker ps
```

**3. Install frontend dependencies**

```bash
cd frontend
npm install
```

**4. Install ML service dependencies**

```bash
cd ../ml-service
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Linux/macOS:

```bash
source .venv/bin/activate
```

Then:

```bash
pip install -r requirements.txt
```

---

## Environment Variables

Create a `.env` file from `.env.example`:

```env
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/studymate"

NEXTAUTH_SECRET="your-secret"

NEXTAUTH_URL="http://localhost:3000"

ML_SERVICE_URL="http://localhost:8000"

EMBEDDING_MODEL="sentence-transformers/all-MiniLM-L6-v2"

EMBEDDING_DIMENSION=384
```

> ⚠️ Never commit `.env` to GitHub.

---

## Database Setup

From the `frontend` directory:

```bash
npx prisma generate
```

Run migrations:

```bash
npx prisma migrate dev
```

Seed the database:

```bash
npx prisma db seed
```

Enable pgvector:

```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

---

## Running the Application

**Start the ML service**

```bash
cd ml-service
uvicorn app.main:app --reload --port 8000
```

The service will run at: `http://localhost:8000`

**Start Next.js**

In another terminal:

```bash
cd frontend
npm run dev
```

The application will be available at: `http://localhost:3000`

---

## API Overview

### Authentication

```
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
```

### Courses

```
GET    /api/courses
POST   /api/courses
GET    /api/courses/:id
PUT    /api/courses/:id
DELETE /api/courses/:id
```

### Units

```
GET  /api/units
POST /api/units
```

### Topics

```
GET  /api/topics
POST /api/topics
```

### Notes

```
GET    /api/notes
POST   /api/notes
GET    /api/notes/:id
DELETE /api/notes/:id
```

### Search

```
GET /api/search
```

Search modes: `keyword`, `semantic`, `hybrid`

### Questions

```
GET    /api/questions
POST   /api/questions
PUT    /api/questions/:id
DELETE /api/questions/:id
```

### Quiz

```
POST /api/quizzes
GET  /api/quizzes/:id
POST /api/quizzes/:id/submit
```

### Analytics

```
GET /api/analytics/student
GET /api/analytics/course
GET /api/analytics/topics
```

---

## Database Search Flow

**Keyword Search**

```
Query → PostgreSQL Full-Text Search → Ranked Results
```

**Semantic Search**

```
Query → Embedding Model → Query Vector → pgvector
      → Similarity Search → Ranked Results
```

**Hybrid Search**

```
Query
 │
 ├───────────────┐
 ▼               ▼
FTS             Embedding
 │               │
 ▼               ▼
Keyword         Vector
Score           Score
 │               │
 └───────┬───────┘
         ▼
   Hybrid Ranking
         ▼
    Final Results
```

---

## Testing

Testing will be performed at multiple levels.

**Database Testing**
- Primary key constraints
- Foreign key constraints
- Unique constraints
- Check constraints
- Transaction rollback
- Stored function behavior
- Trigger behavior
- View correctness
- Full-text search
- Vector search

**API Testing**
- Authentication
- Authorization
- Input validation
- CRUD operations
- Search
- File uploads
- Quiz submission
- Analytics

**Semantic Search Testing**

A manually prepared evaluation dataset will be used to test:

```
Query → Expected Relevant Chunks → Retrieved Top-K Chunks
```

Metrics include: `Precision@K`, `Recall@K`, Search Latency

---

## Performance Evaluation

The following operations will be benchmarked:

```
PDF extraction
Chunking
Embedding generation
Keyword search
Vector search
Hybrid search
Quiz generation
Analytics queries
API response time
```

**Example evaluation table:**

| Operation      | Average Latency | Notes             |
|-----------------|------------------|--------------------|
| Keyword Search  | TBD              | PostgreSQL FTS     |
| Vector Search   | TBD              | pgvector           |
| Hybrid Search   | TBD              | FTS + pgvector     |
| Analytics       | TBD              | SQL aggregation    |
| Quiz Generation | TBD              | PostgreSQL filtering |

*Actual measurements will be added after implementation.*

---

## Security

StudyMate follows basic application and database security practices.

| Area | Practice |
|------|----------|
| Authentication | Passwords are never stored as plaintext |
| Environment Variables | Database credentials and secrets stored via environment variables |
| Authorization | Protected endpoints verify user roles and permissions |
| Input Validation | API input is validated before database operations |
| SQL Injection Protection | Parameterized queries and ORM mechanisms used for all database operations |
| File Upload Security | Uploaded documents validated for file type, size, filename, and storage location |

---

## Git Workflow

The repository uses a feature-based workflow:

```
main
 │
 └── develop
       │
       ├── feature/database
       ├── feature/auth
       ├── feature/notes
       ├── feature/search
       ├── feature/qa
       ├── feature/quiz
       └── feature/analytics
```

**Development rules:**

1. Create a feature branch.
2. Implement one logical feature.
3. Test locally.
4. Commit changes.
5. Push the branch.
6. Open a Pull Request.
7. Review changes.
8. Merge into `develop`.
9. Merge stable releases into `main`.

Commits should clearly represent individual contributions.

---

## Development Roadmap

<details>
<summary><strong>Phase 1 — Requirements</strong></summary>

- Finalize problem statement
- Finalize objectives
- Define scope
- Define functional requirements
- Define non-functional requirements
- Define user roles
</details>

<details>
<summary><strong>Phase 2 — Database Design</strong></summary>

- Design ER diagram
- Define entities, attributes, primary keys, foreign keys, constraints
- Design relational schema
- Design vector data model
</details>

<details>
<summary><strong>Phase 3 — PostgreSQL</strong></summary>

- Set up PostgreSQL and enable pgvector
- Create tables, add constraints and indexes
- Create seed data
- Implement views, stored functions, triggers, transactions
- Implement full-text search
</details>

<details>
<summary><strong>Phase 4 — Backend</strong></summary>

- Initialize Next.js
- Configure Prisma
- Build API layer, authentication, and authorization
- Add input validation
</details>

<details>
<summary><strong>Phase 5 — Document Processing</strong></summary>

- Create FastAPI service
- Implement PDF upload, extraction, and text cleaning
- Implement chunking and metadata extraction
</details>

<details>
<summary><strong>Phase 6 — Embeddings</strong></summary>

- Integrate Sentence Transformers
- Generate and store embeddings
- Configure pgvector and create vector indexes
- Test similarity search
</details>

<details>
<summary><strong>Phase 7 — Search</strong></summary>

- Implement keyword, semantic, and hybrid search
- Implement metadata filtering
- Evaluate search quality and measure latency
</details>

<details>
<summary><strong>Phase 8 — Q&A Bank</strong></summary>

- Create, edit, delete questions
- Add tags and filters
- Embed questions and implement semantic Q&A search
</details>

<details>
<summary><strong>Phase 9 — Quiz</strong></summary>

- Quiz configuration and question selection
- Quiz interface and attempt recording
- Score calculation and transaction handling
- Attempt history
</details>

<details>
<summary><strong>Phase 10 — Analytics</strong></summary>

- Course and topic statistics
- Student performance and weak topic detection
- Dashboard and recommendations
</details>

<details>
<summary><strong>Phase 11 — Testing</strong></summary>

- Database, API, authentication, search, security, performance, and integration tests
</details>

<details>
<summary><strong>Phase 12 — Documentation</strong></summary>

- Architecture and ER diagrams
- Relational schema and data dictionary
- API and SQL documentation
- Testing and performance results
- Screenshots and final report
</details>

---

## Project Documentation

The project documentation will include:

- Problem statement, objectives, and scope
- Functional requirements
- ER diagram and relational schema
- Data dictionary
- Database architecture and technology justification
- PostgreSQL and pgvector implementation
- Semantic and hybrid search methodology
- Important SQL queries and advanced database features
- System and API design
- Application screenshots
- Testing and performance results
- Conclusion and future enhancements

---

## Future Enhancements

**RAG-based Coursework Assistant**

```
Question → Hybrid Retrieval → Relevant Coursework
         → LLM → Grounded Answer → Source Citations
```

**Automatic Question Generation** — Generate questions from uploaded coursework.

**Personalized Recommendations** — Recommend study material based on weak topics, search history, quiz performance, and previous attempts.

**Advanced Retrieval** — Query expansion, reranking, better embedding models, hybrid retrieval optimization, context-aware retrieval.

**Additional Academic Features** — Flashcards, spaced repetition, study sessions, exam preparation modes, course-level knowledge graphs.

> These features are considered future enhancements and are not part of the initial core implementation.

---

## Current Implementation Status

*This section should be updated continuously as development progresses.*

| Component              | Status         |
|--------------------------|----------------|
| Project Requirements     | 🟡 In Progress |
| ER Diagram                | ⬜ Planned     |
| Relational Schema         | ⬜ Planned     |
| PostgreSQL Setup          | ⬜ Planned     |
| pgvector Setup             | ⬜ Planned     |
| Database Schema            | ⬜ Planned     |
| Advanced SQL Features      | ⬜ Planned     |
| Next.js Application        | ⬜ Planned     |
| Authentication              | ⬜ Planned     |
| PDF Processing               | ⬜ Planned     |
| Chunking                       | ⬜ Planned     |
| Embedding Pipeline               | ⬜ Planned     |
| Vector Search                       | ⬜ Planned     |
| Full-Text Search                       | ⬜ Planned     |
| Hybrid Search                             | ⬜ Planned     |
| Q&A Bank                                     | ⬜ Planned     |
| Quiz System                                     | ⬜ Planned     |
| Analytics                                          | ⬜ Planned     |
| Testing                                               | ⬜ Planned     |
| Documentation                                            | ⬜ Planned     |

---

## Project Goals

StudyMate is being developed as a DBMS-focused academic project with emphasis on:

```
Database Design + PostgreSQL + pgvector
    + Semantic Retrieval + Advanced SQL
    + Web Application + Analytics
```

The goal is to demonstrate that database systems can support both structured academic information and modern semantic retrieval workloads within a unified architecture.

---

## Contributors

| Contributor | Responsibilities                  |
|--------------|-------------------------------------|
| TBD           | Database / Backend / Application     |
| TBD           | Database / ML / Application          |

*Both team members will contribute to both the database and application components.*

---

## License

This project is developed for academic and educational purposes. License information will be added when the project license is finalized.

---

## Acknowledgements

StudyMate uses the following open-source technologies and libraries:

- [Next.js](https://nextjs.org/)
- [TypeScript](https://www.typescriptlang.org/)
- [PostgreSQL](https://www.postgresql.org/)
- [pgvector](https://github.com/pgvector/pgvector)
- [Prisma](https://www.prisma.io/)
- [FastAPI](https://fastapi.tiangolo.com/)
- [Python](https://www.python.org/)
- [Sentence Transformers](https://www.sbert.net/)
- [PyMuPDF](https://pymupdf.readthedocs.io/)
- [Tailwind CSS](https://tailwindcss.com/)
- [shadcn/ui](https://ui.shadcn.com/)

---

<div align="center">

**StudyMate**
*Smart Notes. Semantic Search. Better Preparation.*

</div>
