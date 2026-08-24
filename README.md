# StudyMate — Smart Notes & Semantic Q&A Bank for Coursework

> **A coursework knowledge management and semantic retrieval platform powered by PostgreSQL, pgvector, and modern web technologies.**

StudyMate is a web-based academic knowledge management system designed to help students organize coursework, manage study notes, perform semantic and keyword-based searches, maintain a structured Q&A bank, practice quizzes, and analyze their learning performance.

The system combines a **relational PostgreSQL database** with **pgvector-based semantic search**, allowing structured academic information and unstructured coursework content to be managed within a single database architecture.

---

## Table of Contents

* [Overview](#overview)
* [Problem Statement](#problem-statement)
* [Objectives](#objectives)
* [Key Features](#key-features)
* [System Architecture](#system-architecture)
* [End-to-End Data Flow](#end-to-end-data-flow)
* [Technology Stack](#technology-stack)
* [AI/ML Components](#aiml-components)
* [Database Architecture](#database-architecture)
* [Database Schema](#database-schema)
* [Semantic Search Pipeline](#semantic-search-pipeline)
* [Hybrid Search](#hybrid-search)
* [Q&A Bank](#qa-bank)
* [Quiz and Assessment System](#quiz-and-assessment-system)
* [Learning Analytics](#learning-analytics)
* [Authentication and Authorization](#authentication-and-authorization)
* [Project Structure](#project-structure)
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [Environment Variables](#environment-variables)
* [Database Setup](#database-setup)
* [Running the Application](#running-the-application)
* [API Overview](#api-overview)
* [Database Features](#database-features)
* [Testing](#testing)
* [Performance Evaluation](#performance-evaluation)
* [Security](#security)
* [Git Workflow](#git-workflow)
* [Development Roadmap](#development-roadmap)
* [Project Documentation](#project-documentation)
* [Future Enhancements](#future-enhancements)
* [Contributors](#contributors)
* [License](#license)

---

# Overview

Students typically store academic information across multiple disconnected sources such as PDFs, lecture notes, question banks, and personal documents.

Traditional keyword-based search is often insufficient when students remember the **concept** they are looking for but not the exact terminology used in their notes.

StudyMate addresses this problem by combining:

* Structured relational data
* Coursework document management
* PostgreSQL Full-Text Search
* Vector embeddings
* pgvector similarity search
* Hybrid retrieval
* Structured Q&A management
* Quiz-based assessment
* Learning analytics

The system is designed around a central PostgreSQL database, with pgvector providing semantic retrieval capabilities for coursework content.

---

# Problem Statement

Academic study material is often fragmented across lecture notes, PDFs, course units, question banks, and personal resources.

Students face several problems:

1. Difficulty organizing large amounts of coursework.
2. Difficulty finding relevant information using traditional keyword search.
3. Lack of semantic search across their own study material.
4. Difficulty maintaining a structured question bank.
5. Limited visibility into topic-wise academic performance.
6. Lack of a unified platform connecting notes, questions, practice, and analytics.

StudyMate provides a unified platform for managing coursework and retrieving academically relevant information using both traditional database queries and semantic similarity search.

---

# Objectives

The primary objectives of StudyMate are:

* Build a structured academic knowledge management system.
* Store coursework information using a relational database.
* Support document-based academic content.
* Convert coursework content into vector embeddings.
* Store embeddings using PostgreSQL and pgvector.
* Implement semantic similarity search.
* Implement PostgreSQL Full-Text Search.
* Combine keyword and semantic retrieval through hybrid search.
* Build a structured Q&A bank.
* Provide quiz and assessment functionality.
* Track student attempts and performance.
* Identify weak topics.
* Provide learning analytics.
* Demonstrate advanced database concepts beyond basic CRUD.

---

# Key Features

## 1. User Authentication

* Student registration and login.
* Secure password handling.
* Session management.
* Role-based authorization.
* Student and Admin roles.

---

## 2. Course Management

Academic content is organized hierarchically:

```text
Course
   └── Unit
        └── Topic
             └── Notes
```

Example:

```text
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

---

## 3. Notes Management

Students can:

* Upload coursework notes.
* Associate notes with courses.
* Associate notes with units and topics.
* Store note metadata.
* Extract text from PDF documents.
* Divide notes into searchable chunks.

---

## 4. Semantic Search

StudyMate converts coursework chunks into embeddings and stores them in PostgreSQL using pgvector.

A natural-language query can retrieve conceptually relevant material even when the exact words do not appear in the document.

Example:

```text
Query:

"How does a database prevent two transactions from interfering
with each other?"
```

The system can retrieve coursework related to:

```text
Concurrency Control
Isolation
Locking
Serializability
```

---

## 5. Full-Text Search

PostgreSQL Full-Text Search is used for traditional keyword-based retrieval.

This provides strong results for queries containing exact academic terminology.

---

## 6. Hybrid Search

StudyMate combines:

```text
PostgreSQL Full-Text Search
            +
pgvector Semantic Search
            ↓
      Hybrid Ranking
            ↓
      Final Results
```

This allows the system to handle both:

* Exact terminology
* Conceptual queries

---

## 7. Q&A Bank

Questions can be stored and categorized by:

* Course
* Unit
* Topic
* Difficulty
* Question type
* Tags

Supported question types can include:

* MCQ
* True/False
* Short Answer
* Long Answer
* Fill in the Blank

---

## 8. Quiz System

Students can configure quizzes using parameters such as:

```text
Course
Unit
Topic
Difficulty
Number of Questions
```

The system selects appropriate questions from the Q&A bank and records the student's attempts.

---

## 9. Performance Analytics

StudyMate tracks:

* Questions attempted
* Correct answers
* Incorrect answers
* Accuracy
* Topic performance
* Course performance
* Weak topics
* Attempt history

Example:

```text
Topic                  Accuracy
--------------------------------
Normalization             87%
Transactions              61%
Deadlocks                 48%
Indexing                  76%
```

---

## 10. Weak Topic Identification

Student performance is analyzed at the topic level.

```text
Attempts
    ↓
Topic-wise aggregation
    ↓
Accuracy calculation
    ↓
Identify weak topics
    ↓
Retrieve relevant study material
```

This creates a connection between the relational analytics layer and the semantic retrieval layer.

---

# System Architecture

```text
                         ┌─────────────────────┐
                         │       STUDENT       │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       Next.js       │
                         │                     │
                         │ TypeScript          │
                         │ Tailwind CSS        │
                         │ shadcn/ui           │
                         │ Auth.js             │
                         └──────────┬──────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │                               │
                    ▼                               ▼
          ┌───────────────────┐           ┌───────────────────┐
          │ Next.js API       │           │ Python FastAPI    │
          │                   │           │                   │
          │ Authentication    │           │ PDF Processing    │
          │ Courses           │           │ Chunking          │
          │ Notes             │           │ Embeddings        │
          │ Questions         │           │ Semantic Search   │
          │ Quizzes           │           │                   │
          │ Analytics         │           │                   │
          └─────────┬─────────┘           └─────────┬─────────┘
                    │                               │
                    └───────────────┬───────────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     PostgreSQL      │
                         │                     │
                         │ Relational Data     │
                         │                     │
                         │ Users               │
                         │ Courses             │
                         │ Units               │
                         │ Topics              │
                         │ Notes               │
                         │ Questions           │
                         │ Quizzes             │
                         │ Attempts            │
                         │                     │
                         │ Full-Text Search    │
                         │ Views               │
                         │ Functions           │
                         │ Triggers            │
                         │ Transactions        │
                         │                     │
                         │ ┌─────────────────┐ │
                         │ │    pgvector     │ │
                         │ │                 │ │
                         │ │ Embeddings      │ │
                         │ │ Vector Search   │ │
                         │ │ Vector Indexes  │ │
                         │ └─────────────────┘ │
                         └─────────────────────┘
```

---

# End-to-End Data Flow

## Coursework Ingestion

```text
PDF
 │
 ▼
Next.js Upload
 │
 ▼
FastAPI
 │
 ▼
PyMuPDF
 │
 ▼
Text Extraction
 │
 ▼
Text Cleaning
 │
 ▼
Semantic Chunking
 │
 ▼
Embedding Generation
 │
 ▼
Vector Embeddings
 │
 ▼
PostgreSQL + pgvector
```

---

## Search Flow

```text
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
                │
                ▼
          Top-K Results
                │
                ▼
             UI
```

---

## Quiz Flow

```text
Quiz Configuration
       │
       ▼
Question Filtering
       │
       ▼
Question Selection
       │
       ▼
Quiz
       │
       ▼
Student Answers
       │
       ▼
Transaction
       │
       ├── Store Attempt
       ├── Calculate Score
       └── Update Statistics
       │
       ▼
Analytics
```

---

# Technology Stack

| Layer               | Technology                               |
| ------------------- | ---------------------------------------- |
| Frontend            | Next.js                                  |
| Language            | TypeScript                               |
| Styling             | Tailwind CSS                             |
| UI Components       | shadcn/ui                                |
| Application API     | Next.js Route Handlers                   |
| ML API              | Python + FastAPI                         |
| Database            | PostgreSQL                               |
| Vector Extension    | pgvector                                 |
| ORM                 | Prisma                                   |
| Embeddings          | Sentence Transformers                    |
| Embedding Model     | `sentence-transformers/all-MiniLM-L6-v2` |
| Embedding Dimension | 384                                      |
| PDF Processing      | PyMuPDF                                  |
| Search              | PostgreSQL FTS + pgvector                |
| Authentication      | Auth.js                                  |
| Validation          | Zod                                      |
| Containerization    | Docker + Docker Compose                  |
| Database Client     | DBeaver                                  |
| API Testing         | Bruno / Postman                          |
| Version Control     | Git + GitHub                             |

---

# AI/ML Components

## Embedding Model

StudyMate initially uses:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The model converts text into 384-dimensional dense vectors.

```text
Text
 ↓
Sentence Transformer
 ↓
384-dimensional vector
 ↓
pgvector
```

### Why embeddings?

Traditional keyword search requires matching words.

Semantic search instead compares the meaning represented by vectors.

For example:

```text
Query:
"How are simultaneous transactions controlled?"
```

can retrieve content discussing:

```text
Concurrency Control
Locking
Serializability
Isolation
```

even when the exact query wording does not occur in the notes.

---

# Database Architecture

StudyMate uses a single PostgreSQL database with the pgvector extension.

```text
                    PostgreSQL
                         │
          ┌──────────────┴──────────────┐
          │                             │
          ▼                             ▼
   Relational Layer               Vector Layer
          │                             │
          ├── Users                     ├── Note Embeddings
          ├── Courses                   ├── Chunk Embeddings
          ├── Units                     └── Question Embeddings
          ├── Topics
          ├── Notes
          ├── Questions
          ├── Quizzes
          └── Attempts
```

This architecture avoids the need for a separate vector database while keeping structured and semantic data within the same PostgreSQL environment.

---

# Database Schema

The core relational model consists of the following entities:

```text
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

---

## Users

```text
users
--------------------------------
user_id PK
name
email UNIQUE
password_hash
role
created_at
```

---

## Courses

```text
courses
--------------------------------
course_id PK
course_code UNIQUE
course_name
semester
created_at
```

---

## Enrollments

```text
enrollments
--------------------------------
enrollment_id PK
user_id FK
course_id FK
enrolled_at
```

---

## Units

```text
units
--------------------------------
unit_id PK
course_id FK
unit_number
title
description
```

---

## Topics

```text
topics
--------------------------------
topic_id PK
unit_id FK
title
description
```

---

## Notes

```text
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

---

## Note Chunks

```text
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

---

## Questions

```text
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

---

## Attempts

```text
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

# Semantic Search Pipeline

The semantic search system consists of the following stages:

### Step 1 — Document Upload

```text
Student → PDF
```

### Step 2 — Text Extraction

```text
PDF → PyMuPDF → Raw Text
```

### Step 3 — Cleaning

```text
Raw Text → Cleaned Text
```

### Step 4 — Chunking

```text
Cleaned Text → Multiple Chunks
```

### Step 5 — Embedding

```text
Chunk → Sentence Transformer → Vector
```

### Step 6 — Storage

```text
Vector → PostgreSQL pgvector
```

### Step 7 — Query

```text
User Query → Embedding
```

### Step 8 — Similarity Search

```text
Query Vector
     ↓
pgvector
     ↓
Nearest Chunks
```

### Step 9 — Metadata Filtering

Results can be restricted by:

```text
Course
Unit
Topic
Note
```

### Step 10 — Ranking

The most relevant chunks are returned to the user.

---

# Hybrid Search

StudyMate combines two retrieval mechanisms.

## Keyword Retrieval

PostgreSQL Full-Text Search handles exact and lexical matching.

```text
Query
 ↓
PostgreSQL FTS
 ↓
Keyword Results
```

## Semantic Retrieval

pgvector handles semantic similarity.

```text
Query
 ↓
Embedding
 ↓
pgvector
 ↓
Semantic Results
```

## Combined Retrieval

```text
Keyword Score
       +
Semantic Score
       ↓
Hybrid Score
       ↓
Ranked Results
```

Initial ranking can use:

```text
final_score =
    keyword_weight * keyword_score
    +
    semantic_weight * semantic_score
```

The weights will be evaluated experimentally during development.

---

# Q&A Bank

The Q&A system stores structured academic questions.

Each question can contain:

```text
Question
Answer
Course
Unit
Topic
Difficulty
Question Type
Tags
Creator
```

Questions can also be embedded to support semantic question retrieval.

Example:

```text
User Query:

"Questions about database isolation"

        ↓

Question Embedding

        ↓

pgvector

        ↓

Relevant Q&A
```

---

# Quiz and Assessment System

Students can configure a quiz using:

```text
Course
Unit
Topic
Difficulty
Number of Questions
```

The system retrieves matching questions from PostgreSQL.

Example:

```text
Course: DBMS
Unit: 2
Difficulty: Medium
Questions: 10
```

The quiz submission process uses a database transaction to ensure consistent updates.

```text
BEGIN

Create Quiz Attempt
Insert Question Attempts
Calculate Score
Update Statistics

COMMIT
```

If an operation fails:

```text
ROLLBACK
```

---

# Learning Analytics

StudyMate uses SQL aggregation to calculate learning performance.

Example metrics:

```text
Total Questions
Questions Attempted
Correct Answers
Incorrect Answers
Accuracy
Topic Accuracy
Course Accuracy
```

Example SQL concepts demonstrated:

```text
JOIN
GROUP BY
HAVING
COUNT()
SUM()
AVG()
ORDER BY
Subqueries
```

---

# Weak Topic Detection

The system identifies topics where the student's performance falls below a defined threshold.

```text
Attempts
   ↓
GROUP BY Topic
   ↓
Calculate Accuracy
   ↓
Compare with Threshold
   ↓
Weak Topic
```

Example:

```text
Deadlocks → 48%
```

StudyMate can then retrieve:

```text
Relevant Notes
Relevant Questions
Practice Material
```

for the weak topic.

---

# Authentication and Authorization

The application supports two primary roles.

## Student

Students can:

* View enrolled courses.
* View notes.
* Search coursework.
* Access Q&A.
* Take quizzes.
* View their performance.

## Admin

Administrators can:

* Manage users.
* Manage courses.
* Manage units and topics.
* Manage questions.
* Manage tags.
* View system-level statistics.

Authorization is enforced at the API layer.

---

# Database Features

StudyMate intentionally demonstrates advanced database functionality.

## 1. Primary and Foreign Keys

Used to maintain relationships between entities.

## 2. Constraints

Examples:

```text
UNIQUE
NOT NULL
CHECK
FOREIGN KEY
```

## 3. Indexes

Indexes are created for frequently queried fields and vector retrieval.

## 4. Transactions

Used during quiz submission and other multi-step operations.

## 5. Views

Planned views include:

```text
student_topic_performance
student_course_progress
course_question_statistics
topic_question_statistics
```

## 6. Stored Functions

Examples:

```text
get_student_topic_performance()
get_weak_topics()
get_course_statistics()
```

## 7. Triggers

Triggers will be used where database-side consistency or automatic updates are required.

## 8. Full-Text Search

PostgreSQL Full-Text Search provides lexical retrieval.

## 9. Vector Similarity Search

pgvector provides semantic retrieval over coursework embeddings.

---

# Project Structure

```text
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

# Prerequisites

Install the following:

* Node.js
* npm
* Python 3.x
* Docker Desktop
* Git
* VS Code
* DBeaver or another PostgreSQL client

Verify:

```bash
node --version
npm --version
python --version
docker --version
git --version
```

---

# Installation

## 1. Clone the repository

```bash
git clone <repository-url>
cd studymate
```

---

## 2. Start PostgreSQL + pgvector

```bash
docker compose up -d
```

Verify:

```bash
docker ps
```

---

## 3. Install frontend dependencies

```bash
cd frontend
npm install
```

---

## 4. Install ML service dependencies

```bash
cd ../ml-service
python -m venv .venv
```

### Windows

```bash
.venv\Scripts\activate
```

### Linux/macOS

```bash
source .venv/bin/activate
```

Then:

```bash
pip install -r requirements.txt
```

---

# Environment Variables

Create:

```text
.env
```

from:

```text
.env.example
```

Example:

```env
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/studymate"

NEXTAUTH_SECRET="your-secret"

NEXTAUTH_URL="http://localhost:3000"

ML_SERVICE_URL="http://localhost:8000"

EMBEDDING_MODEL="sentence-transformers/all-MiniLM-L6-v2"

EMBEDDING_DIMENSION=384
```

Never commit `.env` to GitHub.

---

# Database Setup

From the frontend directory:

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

# Running the Application

## Start the ML service

```bash
cd ml-service
uvicorn app.main:app --reload --port 8000
```

The service will run at:

```text
http://localhost:8000
```

---

## Start Next.js

In another terminal:

```bash
cd frontend
npm run dev
```

The application will be available at:

```text
http://localhost:3000
```

---

# API Overview

The application will expose APIs approximately as follows:

## Authentication

```text
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
```

## Courses

```text
GET    /api/courses
POST   /api/courses
GET    /api/courses/:id
PUT    /api/courses/:id
DELETE /api/courses/:id
```

## Units

```text
GET  /api/units
POST /api/units
```

## Topics

```text
GET  /api/topics
POST /api/topics
```

## Notes

```text
GET  /api/notes
POST /api/notes
GET  /api/notes/:id
DELETE /api/notes/:id
```

## Search

```text
GET /api/search
```

Search modes:

```text
keyword
semantic
hybrid
```

## Questions

```text
GET    /api/questions
POST   /api/questions
PUT    /api/questions/:id
DELETE /api/questions/:id
```

## Quiz

```text
POST /api/quizzes
GET  /api/quizzes/:id
POST /api/quizzes/:id/submit
```

## Analytics

```text
GET /api/analytics/student
GET /api/analytics/course
GET /api/analytics/topics
```

---

# Database Search Flow

## Keyword Search

```text
Query
 ↓
PostgreSQL Full-Text Search
 ↓
Ranked Results
```

## Semantic Search

```text
Query
 ↓
Embedding Model
 ↓
Query Vector
 ↓
pgvector
 ↓
Similarity Search
 ↓
Ranked Results
```

## Hybrid Search

```text
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

# Testing

Testing will be performed at multiple levels.

## Database Testing

* Primary key constraints.
* Foreign key constraints.
* Unique constraints.
* Check constraints.
* Transaction rollback.
* Stored function behavior.
* Trigger behavior.
* View correctness.
* Full-text search.
* Vector search.

## API Testing

* Authentication.
* Authorization.
* Input validation.
* CRUD operations.
* Search.
* File uploads.
* Quiz submission.
* Analytics.

## Semantic Search Testing

A manually prepared evaluation dataset will be used to test:

```text
Query
Expected Relevant Chunks
Retrieved Top-K Chunks
```

Metrics can include:

```text
Precision@K
Recall@K
Search Latency
```

---

# Performance Evaluation

The following operations will be benchmarked:

```text
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

Example evaluation table:

| Operation       | Average Latency | Notes                |
| --------------- | --------------: | -------------------- |
| Keyword Search  |             TBD | PostgreSQL FTS       |
| Vector Search   |             TBD | pgvector             |
| Hybrid Search   |             TBD | FTS + pgvector       |
| Analytics       |             TBD | SQL aggregation      |
| Quiz Generation |             TBD | PostgreSQL filtering |

Actual measurements will be added after implementation.

---

# Security

StudyMate follows basic application and database security practices.

## Authentication

Passwords are never stored as plaintext.

## Environment Variables

Database credentials and secrets are stored using environment variables.

## Authorization

Protected endpoints verify user roles and permissions.

## Input Validation

API input is validated before database operations.

## SQL Injection Protection

Parameterized queries and ORM mechanisms are used for database operations.

## File Upload Security

Uploaded documents will be validated for:

* File type
* File size
* Filename
* Storage location

---

# Git Workflow

The repository will use a feature-based workflow.

```text
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

Development rules:

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

# Development Roadmap

## Phase 1 — Requirements

* [ ] Finalize problem statement
* [ ] Finalize objectives
* [ ] Define scope
* [ ] Define functional requirements
* [ ] Define non-functional requirements
* [ ] Define user roles

## Phase 2 — Database Design

* [ ] Design ER diagram
* [ ] Define entities
* [ ] Define attributes
* [ ] Define primary keys
* [ ] Define foreign keys
* [ ] Define constraints
* [ ] Design relational schema
* [ ] Design vector data model

## Phase 3 — PostgreSQL

* [ ] Set up PostgreSQL
* [ ] Enable pgvector
* [ ] Create tables
* [ ] Add constraints
* [ ] Add indexes
* [ ] Create seed data
* [ ] Implement views
* [ ] Implement stored functions
* [ ] Implement triggers
* [ ] Implement transactions
* [ ] Implement full-text search

## Phase 4 — Backend

* [ ] Initialize Next.js
* [ ] Configure Prisma
* [ ] Build API layer
* [ ] Build authentication
* [ ] Build authorization
* [ ] Add input validation

## Phase 5 — Document Processing

* [ ] Create FastAPI service
* [ ] Implement PDF upload
* [ ] Implement PDF extraction
* [ ] Implement text cleaning
* [ ] Implement chunking
* [ ] Implement metadata extraction

## Phase 6 — Embeddings

* [ ] Integrate Sentence Transformers
* [ ] Generate embeddings
* [ ] Store embeddings
* [ ] Configure pgvector
* [ ] Create vector indexes
* [ ] Test similarity search

## Phase 7 — Search

* [ ] Implement keyword search
* [ ] Implement semantic search
* [ ] Implement metadata filtering
* [ ] Implement hybrid search
* [ ] Evaluate search quality
* [ ] Measure latency

## Phase 8 — Q&A Bank

* [ ] Create questions
* [ ] Edit questions
* [ ] Delete questions
* [ ] Add tags
* [ ] Filter questions
* [ ] Embed questions
* [ ] Implement semantic Q&A search

## Phase 9 — Quiz

* [ ] Quiz configuration
* [ ] Question selection
* [ ] Quiz interface
* [ ] Attempt recording
* [ ] Score calculation
* [ ] Transaction handling
* [ ] Attempt history

## Phase 10 — Analytics

* [ ] Course statistics
* [ ] Topic statistics
* [ ] Student performance
* [ ] Weak topic detection
* [ ] Dashboard
* [ ] Recommendations

## Phase 11 — Testing

* [ ] Database tests
* [ ] API tests
* [ ] Authentication tests
* [ ] Search tests
* [ ] Security tests
* [ ] Performance tests
* [ ] Integration tests

## Phase 12 — Documentation

* [ ] Architecture diagram
* [ ] ER diagram
* [ ] Relational schema
* [ ] Data dictionary
* [ ] API documentation
* [ ] SQL query documentation
* [ ] Testing results
* [ ] Performance results
* [ ] Screenshots
* [ ] Final report

---

# Project Documentation

The project documentation will include:

* Problem statement
* Objectives
* Scope
* Functional requirements
* ER diagram
* Relational schema
* Data dictionary
* Database architecture
* Technology justification
* PostgreSQL implementation
* pgvector implementation
* Semantic search methodology
* Hybrid search methodology
* Important SQL queries
* Advanced database features
* System architecture
* API design
* Application screenshots
* Testing results
* Performance evaluation
* Conclusion
* Future enhancements

---

# Future Enhancements

Potential future enhancements include:

## RAG-based Coursework Assistant

```text
Question
 ↓
Hybrid Retrieval
 ↓
Relevant Coursework
 ↓
LLM
 ↓
Grounded Answer
 ↓
Source Citations
```

## Automatic Question Generation

Generate questions from uploaded coursework.

## Personalized Recommendations

Recommend study material based on:

* Weak topics
* Search history
* Quiz performance
* Previous attempts

## Advanced Retrieval

Potential future retrieval improvements include:

* Query expansion
* Reranking
* Better embedding models
* Hybrid retrieval optimization
* Context-aware retrieval

## Additional Academic Features

* Flashcards
* Spaced repetition
* Study sessions
* Exam preparation modes
* Course-level knowledge graphs

These features are considered future enhancements and are not part of the initial core implementation.

---

# Current Implementation Status

> This section should be updated continuously as development progresses.

| Component             | Status         |
| --------------------- | -------------- |
| Project Requirements  | 🟡 In Progress |
| ER Diagram            | ⬜ Planned      |
| Relational Schema     | ⬜ Planned      |
| PostgreSQL Setup      | ⬜ Planned      |
| pgvector Setup        | ⬜ Planned      |
| Database Schema       | ⬜ Planned      |
| Advanced SQL Features | ⬜ Planned      |
| Next.js Application   | ⬜ Planned      |
| Authentication        | ⬜ Planned      |
| PDF Processing        | ⬜ Planned      |
| Chunking              | ⬜ Planned      |
| Embedding Pipeline    | ⬜ Planned      |
| Vector Search         | ⬜ Planned      |
| Full-Text Search      | ⬜ Planned      |
| Hybrid Search         | ⬜ Planned      |
| Q&A Bank              | ⬜ Planned      |
| Quiz System           | ⬜ Planned      |
| Analytics             | ⬜ Planned      |
| Testing               | ⬜ Planned      |
| Documentation         | ⬜ Planned      |

---

# Project Goals

StudyMate is being developed as a **DBMS-focused academic project** with emphasis on:

```text
Database Design
       +
PostgreSQL
       +
pgvector
       +
Semantic Retrieval
       +
Advanced SQL
       +
Web Application
       +
Analytics
```

The goal is to demonstrate that database systems can support both structured academic information and modern semantic retrieval workloads within a unified architecture.

---

# Contributors

| Contributor | Responsibilities                 |
| ----------- | -------------------------------- |
| TBD         | Database / Backend / Application |
| TBD         | Database / ML / Application      |

Both team members will contribute to both the database and application components.

---

# License

This project is developed for academic and educational purposes.

License information will be added when the project license is finalized.

---

# Acknowledgements

StudyMate uses the following open-source technologies and libraries:

* Next.js
* TypeScript
* PostgreSQL
* pgvector
* Prisma
* FastAPI
* Python
* Sentence Transformers
* PyMuPDF
* Tailwind CSS
* shadcn/ui

---

## StudyMate

**Smart Notes. Semantic Search. Better Preparation.**
