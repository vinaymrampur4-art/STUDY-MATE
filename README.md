<div align="center">

# 📚 StudyMate

### Smart Notes & Semantic Q&A Bank for Coursework

**A coursework knowledge management platform with semantic search powered by PostgreSQL and pgvector.**

</div>

StudyMate is a web-based academic knowledge management system designed to help students organize, upload, manage, and search their coursework material.

Unlike a traditional file repository that relies only on folders and keyword matching, StudyMate uses **vector embeddings and semantic similarity search** to understand the meaning of notes and previous-year questions (PYQs).

Students can search using natural-language queries such as:

> *"notes about transaction isolation levels"*

and retrieve relevant coursework even when the exact words used in the query do not appear in the stored material.

The system combines **relational database management, PostgreSQL Full-Text Search, vector similarity search, role-based access control, and academic content organization** in a single application.

---

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Motivation](#motivation)
- [Objectives](#objectives)
- [Scope](#scope)
- [Out of Scope for Initial Version](#out-of-scope-for-initial-version)
- [Key Features](#key-features)
- [User Roles](#user-roles)
- [System Architecture](#system-architecture)
- [End-to-End Data Flow](#end-to-end-data-flow)
- [Document Processing Pipeline](#document-processing-pipeline)
- [AI and Embedding Model](#ai-and-embedding-model)
- [Database Architecture](#database-architecture)
- [Database Schema](#database-schema)
- [Entity Relationship Overview](#entity-relationship-overview)
- [Core Database Models](#core-database-models)
- [Semantic Search Pipeline](#semantic-search-pipeline)
- [Similarity Search](#similarity-search)
- [Metadata Filtering](#metadata-filtering)
- [Q&A and PYQ Bank](#qa-and-pyq-bank)
- [Authentication and Authorization](#authentication-and-authorization)
- [Role-Based Access](#role-based-access)
- [Advanced Database Features](#advanced-database-features)
- [API Architecture](#api-architecture)
- [API Overview](#api-overview)
- [Project Structure](#project-structure)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
- [Development Workflow](#development-workflow)
- [Testing](#testing)
- [Semantic Search Evaluation](#semantic-search-evaluation)
- [Performance Evaluation](#performance-evaluation)
- [Security](#security)
- [Git and GitHub Workflow](#git-and-github-workflow)
- [Development Roadmap](#development-roadmap)
- [Project Documentation](#project-documentation)
- [Future Enhancements](#future-enhancements)
- [Current Status](#current-status)
- [Project Timeline](#project-timeline)
- [Project Goals](#project-goals)
- [Contributors](#contributors)
- [License](#license)
- [Acknowledgements](#acknowledgements)

---

## Overview

StudyMate provides a centralized platform for managing academic coursework.

The system organizes study material by:

```
Course
   ↓
Unit
   ↓
Topic
   ↓
Study Material
```

Study material can include:

- Lecture notes
- Previous-year questions (PYQs)
- Reference material
- Other approved coursework documents

The central feature of the system is semantic retrieval:

```
Student Query
      ↓
Embedding Generation
      ↓
Vector Similarity Search
      ↓
Relevant Coursework
```

StudyMate also supports traditional keyword retrieval through PostgreSQL Full-Text Search. The two approaches can be combined to provide hybrid search.

---

## Problem Statement

Students commonly store coursework across:

- PDF files
- Shared folders
- Messaging applications
- Cloud drives
- Previous-year question documents
- Personal notes

As the amount of study material increases, manually organizing and searching this information becomes difficult.

Traditional keyword search also has an important limitation: the search query must often contain words that actually occur in the stored document.

**For example:**

> Query: *"How are simultaneous transactions controlled?"*

A traditional keyword search may fail to retrieve material containing:

```
Concurrency Control
Isolation
Serializability
Locking
```

even though those topics are conceptually related. StudyMate addresses this problem by using **semantic vector search** over coursework content.

---

## Motivation

The primary motivation behind StudyMate is to make academic material easier to discover and retrieve.

**Faster Coursework Search** — Students can search using natural language rather than remembering exact terminology.

**Centralized Study Material** — Notes and previous-year questions can be organized by course and topic.

**Semantic Understanding** — Vector embeddings allow the system to retrieve content based on semantic similarity rather than exact keyword matching.

**Database-Centric Architecture** — Structured information such as users, courses, roles, submissions, and metadata is stored relationally, while unstructured academic content is converted into embeddings and searched using pgvector.

This provides a genuine justification for using:

```
PostgreSQL + pgvector
```

rather than adding a separate NoSQL database simply for technology demonstration.

---

## Objectives

1. Build a centralized coursework management system.
2. Organize academic material by course, unit, and topic.
3. Support Student and Admin roles.
4. Allow students to upload and manage study material.
5. Allow administrators to manage courses and review uploads.
6. Extract text from uploaded coursework documents.
7. Divide extracted content into searchable chunks.
8. Generate vector embeddings for academic content.
9. Store embeddings using PostgreSQL and pgvector.
10. Implement semantic similarity search.
11. Implement PostgreSQL Full-Text Search.
12. Implement hybrid keyword + semantic search.
13. Support searching across notes and PYQs.
14. Demonstrate advanced database concepts.
15. Maintain a clean separation between structured and unstructured data.
16. Provide a functional Next.js web application.

---

## Scope

### Included in the Core System

**User Management**
- Student registration/login
- Admin login
- Role-based access

**Course Management**
- Courses, Units, Topics
- Course enrollment/access

**Study Material**
- Notes, PYQs, Reference material
- Uploads
- Course/topic tagging
- Material review

**Search**
- Keyword search
- Semantic search
- Hybrid search
- Course / Unit / Topic / Material-type filtering

**Database**
- PostgreSQL
- pgvector
- Primary/foreign keys, constraints, indexes
- Transactions
- Stored functions
- Views
- Full-text search
- Vector similarity search

---

## Out of Scope for Initial Version

The following are intentionally not part of the initial core implementation:

- Full LMS functionality
- Complex quiz engine
- Voice assistant
- Mobile application
- Large-scale recommendation engine
- Microservice infrastructure
- MongoDB
- Redis
- Elasticsearch
- Separate vector database
- Kubernetes
- LLM-based chatbot

These may be considered future enhancements after the core system is complete.

---

## Key Features

### 1. Course-Wise Organization

Study material is organized using:

```
Course
 ├── Unit
 │    ├── Topic
 │    └── Topic
 │
 └── Unit
      ├── Topic
      └── Topic
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

### 2. Study Material Management

StudyMate uses a generic study material model that supports multiple material types:

```
Study Material
│
├── NOTE
├── PYQ
└── REFERENCE
```

Each material item can be associated with Course, Unit, Topic, Uploader, Material Type, File, Metadata, and Upload Date.

### 3. Upload and Review

Students can upload permitted study material. The upload process associates the material with the appropriate academic hierarchy:

```
Upload → Course → Unit → Topic → Study Material
```

Administrators can review uploaded material and manage the academic content repository.

### 4. Semantic Search

Semantic search is the main intelligent feature of StudyMate.

**Example query:** *"notes about transaction isolation levels"*

```
User Query
    ↓
Embedding Model
    ↓
Query Vector
    ↓
pgvector
    ↓
Similarity Search
    ↓
Top-K Relevant Chunks
    ↓
Metadata Filtering
    ↓
Search Results
```

The search is based on semantic similarity rather than exact keyword matching.

### 5. Keyword Search

StudyMate also supports PostgreSQL Full-Text Search:

```
User Query → PostgreSQL Full-Text Search → Ranked Keyword Results
```

This is particularly useful when users know the exact academic terminology.

### 6. Hybrid Search

StudyMate combines semantic and keyword retrieval:

```
                         Query
                           │
                ┌──────────┴──────────┐
                │                     │
                ▼                     ▼
        PostgreSQL FTS          Query Embedding
                │                     │
                ▼                     ▼
        Keyword Results         pgvector Search
                │                     │
                └──────────┬──────────┘
                           ▼
                    Hybrid Ranking
                           │
                           ▼
                    Final Results
```

The initial hybrid ranking uses a weighted score:

```
final_score =
    keyword_weight × keyword_score
    +
    semantic_weight × semantic_score
```

The weights can be evaluated and adjusted during testing.

---

## User Roles

### Student

- Log in
- View available courses
- Access course material
- Search notes and PYQs
- Upload permitted study material
- Filter search results
- View relevant coursework

### Admin

- Manage users
- Manage courses
- Manage units
- Manage topics
- Review uploads
- Manage study material
- Manage questions/PYQs

---

## System Architecture

```
                         ┌─────────────────────┐
                         │       STUDENT        │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       Next.js         │
                         │                       │
                         │ TypeScript             │
                         │ Tailwind CSS           │
                         │ shadcn/ui              │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      API Routes     │
                         │                     │
                         │ JWT Authentication  │
                         │ Courses             │
                         │ Units               │
                         │ Topics              │
                         │ Materials           │
                         │ Search              │
                         │ Admin               │
                         └──────────┬──────────┘
                                    │
                    ┌───────────────┴──────────────-─┐
                    │                                │
                    ▼                                ▼
          ┌───────────────────┐             ┌───────────────────--┐
          │    PostgreSQL       │           │   Python/FastAPI    │
          │                     │           │                     │
          │ Users               │           │ PDF Processing      │
          │ Courses             │           │ Text Extraction     │
          │ Units               │           │ Chunking            │
          │ Topics              │           │ Embeddings          │
          │ Materials           │           │                     │
          │ Questions           │           └─────────┬───────────┘
          │ Uploads             │                     │
          │                     │                     │
          │ Full-Text Search    │                     │
          │ Views               │                     │
          │ Functions           │                     │
          │ Transactions        │                     │
          │                     │                     │
          │   ┌───────────┐     │                     │
          │   │ pgvector  │◄────┼─────────────────────┘
          │   └───────────┘     │
          └──────────────────--─┘
```

---

## End-to-End Data Flow

### Document Ingestion

```
PDF / Study Material
        ↓
Next.js Upload
        ↓
API
        ↓
Python FastAPI
        ↓
PyMuPDF
        ↓
Text Extraction
        ↓
Text Cleaning
        ↓
Chunking
        ↓
Sentence Transformer
        ↓
Embedding Vectors
        ↓
PostgreSQL + pgvector
```

---

## Document Processing Pipeline

| Step | Description |
|------|--------------|
| 1. Upload | The user uploads a supported study material file (`PDF → Upload API`) |
| 2. Metadata | The material is associated with Course, Unit, Topic, Uploader, and Material Type |
| 3. Text Extraction | PyMuPDF extracts text from the document (`PDF → Raw Text`) |
| 4. Cleaning | The extracted content is normalized to remove unnecessary formatting and noise |
| 5. Chunking | Large documents are divided into smaller chunks (Chunk 1, Chunk 2, Chunk 3, ...) |
| 6. Embedding Generation | Each chunk is converted into a 384-dimensional vector via a Sentence Transformer |
| 7. Storage | The chunk and its embedding are stored in `study_material_chunks` |

**Storage schema:**

```
study_material_chunks
----------------------------
chunk_id
material_id
chunk_text
chunk_index
page_number
embedding
created_at
```

---

## AI and Embedding Model

### Embedding Model

The initial implementation uses:

```
sentence-transformers/all-MiniLM-L6-v2
```

**Characteristics:**

```
Model:              all-MiniLM-L6-v2
Embedding Dimension: 384
Purpose:             Semantic text representation
```

The model converts text into dense numerical vectors that can be compared using vector similarity.

### Why Sentence Transformers?

The initial project uses a local pre-trained embedding model because it:

- Does not require a paid API
- Can run locally
- Does not require external API credentials
- Is lightweight enough for a student development environment
- Integrates directly with pgvector

---

## Database Architecture

StudyMate uses:

```
PostgreSQL + pgvector
```

instead of PostgreSQL plus a separate NoSQL database. The reasoning is:

```
Structured Data → PostgreSQL
```

Examples: Users, Roles, Courses, Units, Topics, Uploads, Metadata

```
Unstructured Academic Text → Embeddings → pgvector
```

Examples: Note chunks, PYQ text, Question text

This provides a functional reason for the SQL + Vector architecture.

---

## Database Schema

The initial database consists of the following core entities:

```
users
courses
enrollments

units
topics

study_material
study_material_chunks

questions
tags
question_tags

uploads
```

---

## Entity Relationship Overview

```
USER
 │
 ├──────────────< ENROLLMENT >────────────── COURSE
 │                                             │
 │                                             ├── UNIT
 │                                             │     │
 │                                             │     └── TOPIC
 │                                             │           │
 │                                             │           └── STUDY_MATERIAL
 │                                             │                    │
 │                                             │                    └── CHUNK
 │                                             │                         │
 │                                             │                         └── EMBEDDING
 │                                             │
 │                                             └── QUESTION
 │                                                   │
 │                                                   └── TAG
 │
 └──< UPLOAD >── STUDY_MATERIAL
```

---

## Core Database Models

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

Possible roles: `STUDENT`, `ADMIN`

### Courses

```
courses
--------------------------------
course_id PK
course_code UNIQUE
course_name
description
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

### Study Material

```
study_material
--------------------------------
material_id PK
course_id FK
unit_id FK
topic_id FK
uploaded_by FK
title
material_type
file_name
file_path
status
created_at
```

Possible material types: `NOTE`, `PYQ`, `REFERENCE`
Possible review states: `PENDING`, `APPROVED`, `REJECTED`

### Study Material Chunks

```
study_material_chunks
--------------------------------
chunk_id PK
material_id FK
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
unit_id FK
topic_id FK
question_text
answer
question_type
difficulty
created_by FK
created_at
```

### Tags

```
tags
--------------------------------
tag_id PK
tag_name UNIQUE
```

### Question Tags

```
question_tags
--------------------------------
question_id FK
tag_id FK
```

---

## Semantic Search Pipeline

```
                         User Query
                              │
                              ▼
                     Query Embedding
                              │
                              ▼
                    384-Dimensional Vector
                              │
                              ▼
                         pgvector
                              │
                              ▼
                     Similarity Search
                              │
                              ▼
                         Top-K Chunks
                              │
                              ▼
                     Metadata Filtering
                              │
                              ▼
                       Ranked Results
```

---

## Similarity Search

The system compares:

```
Query Vector  vs.  Stored Chunk Vectors
```

and retrieves the nearest relevant vectors according to the selected similarity metric. The implementation will use an appropriate pgvector index once the expected dataset size and retrieval requirements are finalized.

---

## Metadata Filtering

Semantic results can be restricted using relational metadata.

**Example:**

```
Course = DBMS
Unit = Unit 3
Topic = Transactions
Material Type = NOTE
```

Conceptually:

```sql
WHERE course_id = ?
AND unit_id = ?
AND topic_id = ?
```

combined with vector similarity ranking. This demonstrates how relational filtering and vector retrieval can work together.

---

## Q&A and PYQ Bank

StudyMate provides a structured question repository. Questions can be organized by Course, Unit, Topic, Difficulty, Question Type, and Tags.

**Example:**

```
Question:   What is serializability?
Course:     DBMS
Unit:       Transactions
Topic:      Concurrency Control
Type:       Short Answer
Difficulty: Medium
```

Questions can also be embedded to support semantic search across the Q&A/PYQ bank.

---

## Authentication and Authorization

StudyMate uses JWT-based authentication.

### Login Flow

```
User → Login API → Verify Credentials → Generate JWT → Return Token
```

Protected requests include the token:

```
Authorization: Bearer <JWT>
```

The API verifies:

```
JWT → User → Role → Permission
```

---

## Role-Based Access

### Student

```
View Courses
Search Materials
View Notes
View PYQs
Upload Permitted Material
```

### Admin

```
Manage Users
Manage Courses
Manage Units
Manage Topics
Review Uploads
Manage Study Material
Manage Questions
```

---

## Advanced Database Features

StudyMate is designed to demonstrate database concepts beyond basic CRUD.

### 1. Primary and Foreign Keys

Used to represent relationships between academic entities.

### 2. Constraints

```
PRIMARY KEY
FOREIGN KEY
UNIQUE
NOT NULL
CHECK
```

### 3. Indexing

Indexes will be created on frequently queried attributes such as:

```
users.email
courses.course_code
enrollments.user_id
enrollments.course_id
units.course_id
topics.unit_id
study_material.course_id
study_material.topic_id
```

Vector indexes will be used for semantic retrieval.

### 4. Transactions

Uploading study material involves multiple related operations:

```
BEGIN TRANSACTION

Insert Material
Insert Metadata
Process/Store Chunks
Associate Material

COMMIT
```

If a required operation fails:

```
ROLLBACK
```

This ensures the database does not contain incomplete metadata.

### 5. Stored Functions

Potential database functions include:

```
check_course_access()
check_material_access()
check_user_enrollment()
```

Additional functions will be added where they provide meaningful database-side functionality.

### 6. Views

Potential views include:

```
course_material_summary
student_course_access
material_search_summary
question_statistics
```

### 7. Full-Text Search

PostgreSQL Full-Text Search provides keyword-based retrieval.

### 8. Vector Similarity Search

pgvector provides semantic retrieval over embedded academic content.

---

## API Architecture

```
Frontend
   ↓
Next.js API Route
   ↓
Validation
   ↓
Authorization
   ↓
Database / ML Service
   ↓
Response
```

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
GET    /api/units
POST   /api/units
PUT    /api/units/:id
DELETE /api/units/:id
```

### Topics

```
GET    /api/topics
POST   /api/topics
PUT    /api/topics/:id
DELETE /api/topics/:id
```

### Study Material

```
GET    /api/materials
POST   /api/materials
GET    /api/materials/:id
PUT    /api/materials/:id
DELETE /api/materials/:id
```

### Search

```
GET /api/search
```

Supported modes: `keyword`, `semantic`, `hybrid`

### Questions

```
GET    /api/questions
POST   /api/questions
GET    /api/questions/:id
PUT    /api/questions/:id
DELETE /api/questions/:id
```

### Admin

```
GET /api/admin/users
GET /api/admin/uploads
PUT /api/admin/uploads/:id/review
```

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
│   │   │   ├── materials/
│   │   │   ├── questions/
│   │   │   ├── search/
│   │   │   └── admin/
│   │   │
│   │   ├── login/
│   │   ├── register/
│   │   ├── dashboard/
│   │   ├── courses/
│   │   ├── materials/
│   │   ├── search/
│   │   ├── questions/
│   │   ├── admin/
│   │   └── ...
│   │
│   ├── components/
│   ├── lib/
│   ├── middleware.ts
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
│   ├── requirements/
│   ├── er-diagram/
│   ├── architecture/
│   ├── database/
│   ├── api/
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

## Technology Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| Frontend | Next.js | Web application |
| Language | TypeScript | Application development |
| UI | Tailwind CSS | Styling |
| Components | shadcn/ui | UI components |
| Backend/API | Next.js API Routes | Application API |
| Authentication | JWT | Student/Admin authentication |
| Primary DB | PostgreSQL | Structured data |
| Vector DB | PostgreSQL + pgvector | Embeddings and similarity search |
| ORM | Prisma | Database access |
| Raw SQL | PostgreSQL SQL | Advanced database operations |
| Document Processing | PyMuPDF | PDF text extraction |
| ML | Python | Embedding processing |
| Embeddings | Sentence Transformers | Text embeddings |
| Embedding Model | all-MiniLM-L6-v2 | 384-dimensional embeddings |
| Keyword Search | PostgreSQL FTS | Lexical retrieval |
| Semantic Search | pgvector | Similarity retrieval |
| Validation | Zod | API validation |
| Containerization | Docker Compose | Development environment |
| Version Control | Git + GitHub | Source control |
| Database Tool | DBeaver | PostgreSQL development |
| API Testing | Postman / Bruno | API testing |

---

## Prerequisites

Install the following software:

- Node.js
- npm
- Python 3.x
- Docker Desktop
- Git
- VS Code
- DBeaver

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

**1. Clone the Repository**

```bash
git clone <repository-url>
cd studymate
```

**2. Start PostgreSQL + pgvector**

```bash
docker compose up -d
```

Verify that the container is running:

```bash
docker ps
```

**3. Install Frontend Dependencies**

```bash
cd frontend
npm install
```

**4. Set Up Python Environment**

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

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Environment Variables

Create a `.env` file based on `.env.example`:

```env
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/studymate"

JWT_SECRET="your-secret-key"

ML_SERVICE_URL="http://localhost:8000"

EMBEDDING_MODEL="sentence-transformers/all-MiniLM-L6-v2"

EMBEDDING_DIMENSION=384
```

> ⚠️ Never commit `.env` to GitHub.

---

## Database Setup

Start the PostgreSQL container:

```bash
docker compose up -d
```

Enable pgvector:

```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

Generate Prisma client:

```bash
cd frontend
npx prisma generate
```

Run database migrations:

```bash
npx prisma migrate dev
```

Seed development data:

```bash
npx prisma db seed
```

---

## Running the Application

**Start the Python ML Service**

```bash
cd ml-service
uvicorn app.main:app --reload --port 8000
```

The ML service runs at: `http://localhost:8000`

**Start the Next.js Application**

Open another terminal:

```bash
cd frontend
npm run dev
```

The application runs at: `http://localhost:3000`

---

## Development Workflow

```
Requirements
     ↓
ER Diagram
     ↓
Relational Schema
     ↓
PostgreSQL + pgvector
     ↓
Database Implementation
     ↓
Advanced DB Features
     ↓
Next.js Application
     ↓
JWT Authentication
     ↓
Course Management
     ↓
Material Upload
     ↓
PDF Processing
     ↓
Chunking
     ↓
Embeddings
     ↓
Vector Search
     ↓
Full-Text Search
     ↓
Hybrid Search
     ↓
Q&A/PYQ Search
     ↓
Testing
     ↓
Performance Evaluation
     ↓
Documentation
     ↓
Final Demonstration
```

---

## Testing

Testing will cover multiple layers.

**Database Testing**
- Primary key constraints
- Foreign key constraints
- Unique constraints
- Check constraints
- Transaction rollback
- Stored functions
- Views
- Indexes
- Full-text search
- Vector similarity search

**API Testing**

Test registration, login, JWT validation, role authorization, and all Course / Unit / Topic / Material / Search / Question / Admin APIs.

**File Processing Testing**

Test valid PDFs, invalid files, empty documents, large documents, text extraction, chunking, and embedding generation.

**Search Testing**

Test exact keyword queries, semantic queries, course-filtered queries, topic-filtered queries, material-type filters, and hybrid queries.

---

## Semantic Search Evaluation

A small manually prepared evaluation dataset will be used.

**Example:**

| Query | Expected Relevant Content |
|-------|----------------------------|
| What is transaction isolation? | Transaction Isolation |
| How do concurrent transactions interact? | Concurrency Control |
| What reduces redundancy in relations? | Normalization |
| What is a deadlock? | Deadlock |

Metrics include: `Precision@K`, `Recall@K`, Search Latency

---

## Performance Evaluation

The following operations will be measured:

```
PDF extraction time
Chunking time
Embedding generation time
Keyword search latency
Vector search latency
Hybrid search latency
API response time
Database query performance
```

**Example:**

| Operation | Average Latency | Method |
|-----------|-----------------:|--------|
| Keyword Search | TBD | PostgreSQL FTS |
| Semantic Search | TBD | pgvector |
| Hybrid Search | TBD | FTS + pgvector |
| Material Retrieval | TBD | PostgreSQL |
| Access Check | TBD | SQL Function |

*Actual benchmark values will be added after implementation.*

---

## Security

StudyMate follows basic database and web security practices.

| Area | Practice |
|------|----------|
| Password Security | Passwords must never be stored in plaintext |
| JWT Security | JWT secrets must be stored in environment variables |
| Database Credentials | Database credentials must never be hard-coded |
| Input Validation | API input is validated before database operations |
| SQL Injection Protection | Parameterized queries and Prisma mechanisms are used for normal database operations; raw SQL must be carefully parameterized |
| Role-Based Authorization | Admin-only operations must verify the authenticated user's role |
| File Upload Security | Uploaded documents validated for file type, size, filename, and storage path |

---

## Git and GitHub Workflow

The project uses Git and GitHub for version control.

**Recommended branch structure:**

```
main
 │
 └── develop
      │
      ├── feature/database
      ├── feature/auth
      ├── feature/courses
      ├── feature/materials
      ├── feature/search
      ├── feature/qa
      └── feature/admin
```

**Recommended workflow:**

```
Create Feature Branch
        ↓
Implement Feature
        ↓
Test
        ↓
Commit
        ↓
Push
        ↓
Pull Request
        ↓
Code Review
        ↓
Merge into develop
        ↓
Release to main
```

Commits should clearly demonstrate individual contributions from both team members.

---

## Development Roadmap

<details>
<summary><strong>Phase 1 — Project Definition</strong></summary>

- [ ] Finalize problem statement
- [ ] Finalize objectives
- [ ] Define scope
- [ ] Define user roles
- [ ] Define functional requirements
- [ ] Define non-functional requirements
- [ ] Freeze technology stack
</details>

<details>
<summary><strong>Phase 2 — Database Design</strong></summary>

- [ ] Identify entities and attributes
- [ ] Define primary keys, foreign keys, and constraints
- [ ] Create ER diagram
- [ ] Create relational schema
- [ ] Design pgvector data model
- [ ] Create data dictionary
</details>

<details>
<summary><strong>Phase 3 — Database Implementation</strong></summary>

- [ ] Set up PostgreSQL and enable pgvector
- [ ] Create tables, relationships, constraints, indexes
- [ ] Create seed data
- [ ] Create views, stored functions, triggers
- [ ] Implement transactions
- [ ] Implement Full-Text Search
- [ ] Implement vector indexes
</details>

<details>
<summary><strong>Phase 4 — Backend</strong></summary>

- [ ] Initialize Next.js and configure Prisma
- [ ] Build API architecture and validation
- [ ] Implement JWT authentication and authorization middleware
- [ ] Implement course, material, question, search, and admin APIs
</details>

<details>
<summary><strong>Phase 5 — Document Processing</strong></summary>

- [ ] Create Python FastAPI service
- [ ] Implement PDF upload handling
- [ ] Implement text extraction and cleaning
- [ ] Implement chunking and metadata handling
</details>

<details>
<summary><strong>Phase 6 — Embeddings</strong></summary>

- [ ] Integrate Sentence Transformers
- [ ] Load `all-MiniLM-L6-v2`
- [ ] Generate and store embeddings
- [ ] Configure pgvector and create vector indexes
- [ ] Test vector similarity search
</details>

<details>
<summary><strong>Phase 7 — Search</strong></summary>

- [ ] Implement keyword, semantic, and hybrid search
- [ ] Implement metadata filtering and result ranking
- [ ] Evaluate search quality and measure latency
</details>

<details>
<summary><strong>Phase 8 — Q&A / PYQ Bank</strong></summary>

- [ ] Create question model and management
- [ ] Add question tags and filtering
- [ ] Add question embeddings
- [ ] Implement semantic question search
</details>

<details>
<summary><strong>Phase 9 — Frontend</strong></summary>

- [ ] Login / registration pages
- [ ] Dashboard
- [ ] Course page and unit/topic navigation
- [ ] Material upload and listing
- [ ] Search interface, filters, and results
- [ ] Q&A/PYQ interface
- [ ] Admin dashboard
</details>

<details>
<summary><strong>Phase 10 — Testing</strong></summary>

- [ ] Database, API, authentication, and authorization tests
- [ ] Upload, search, and vector retrieval tests
- [ ] Security, integration, and performance tests
</details>

<details>
<summary><strong>Phase 11 — Documentation</strong></summary>

- [ ] Problem statement, objectives, scope, and requirements
- [ ] ER diagram, relational schema, data dictionary
- [ ] Database justification, SQL and vector implementation
- [ ] Search methodology, architecture, API documentation
- [ ] Screenshots, testing and performance results
- [ ] Conclusion and future enhancements
</details>

---

## Project Documentation

The final project documentation will contain:

1. Problem Statement
2. Motivation
3. Objectives
4. Scope
5. Functional Requirements
6. Non-Functional Requirements
7. User Roles
8. Technology Stack
9. Technology Justification
10. System Architecture
11. ER Diagram
12. Relational Schema
13. Data Dictionary
14. PostgreSQL Implementation
15. pgvector Implementation
16. Document Processing Pipeline
17. Embedding Model
18. Semantic Search
19. Full-Text Search
20. Hybrid Search
21. Important SQL Queries
22. Advanced Database Features
23. API Architecture
24. Authentication
25. Application Screenshots
26. Testing
27. Performance Evaluation
28. Results
29. Limitations
30. Future Enhancements
31. Conclusion

---

## Future Enhancements

The following features may be added after the core system is stable.

**RAG-Based Coursework Assistant**

```
Student Question → Hybrid Search → Relevant Coursework
       → LLM → Grounded Answer → Source References
```

**Automatic Question Generation** — Generate questions from uploaded coursework.

**Personalized Recommendations** — Recommend material based on search history, course activity, weak topics, and frequently accessed material.

**Quiz System** — Generate quizzes from Course, Unit, Topic, and Difficulty.

**Learning Analytics** — Track search activity, material usage, question attempts, and topic performance.

**Advanced Retrieval** — Query expansion, result reranking, better embedding models, hybrid search optimization, cross-encoder reranking.

> These features are not required for the initial core implementation.

---

## Current Status

*This section should be updated throughout development.*

| Component | Status |
|-----------|--------|
| Project Definition | 🟢 Defined |
| Technology Stack | 🟢 Defined |
| ER Diagram | 🟡 In Progress |
| Relational Schema | 🟡 In Progress |
| PostgreSQL Setup | ⬜ Planned |
| pgvector Setup | ⬜ Planned |
| Database Schema | ⬜ Planned |
| Database Functions | ⬜ Planned |
| Database Views | ⬜ Planned |
| Database Triggers | ⬜ Planned |
| Transactions | ⬜ Planned |
| Next.js Application | ⬜ Planned |
| JWT Authentication | ⬜ Planned |
| Course Management | ⬜ Planned |
| Material Management | ⬜ Planned |
| PDF Processing | ⬜ Planned |
| Chunking | ⬜ Planned |
| Embedding Pipeline | ⬜ Planned |
| Semantic Search | ⬜ Planned |
| Full-Text Search | ⬜ Planned |
| Hybrid Search | ⬜ Planned |
| Q&A/PYQ Bank | ⬜ Planned |
| Admin Dashboard | ⬜ Planned |
| Testing | ⬜ Planned |
| Performance Evaluation | ⬜ Planned |
| Final Documentation | ⬜ Planned |

---

## Project Timeline

The initial target is approximately **4–5 weeks** for a complete academic implementation by a two-member team.

```
Week 1
├── Requirements
├── ER Diagram
├── Relational Schema
└── PostgreSQL Setup

Week 2
├── Database Implementation
├── Constraints
├── Indexes
├── Views
├── Functions
├── Transactions
└── Full-Text Search

Week 3
├── Next.js
├── JWT Authentication
├── Course Management
├── Material Management
└── Python Processing Service

Week 4
├── PDF Processing
├── Chunking
├── Embeddings
├── pgvector
├── Semantic Search
└── Hybrid Search

Week 5
├── Q&A/PYQ Bank
├── Admin Features
├── Integration Testing
├── Performance Testing
├── Documentation
└── Final Demonstration
```

---

## Project Goals

StudyMate is designed to demonstrate the integration of:

```
              STUDYMATE
                  │
      ┌───────────┼───────────┐
      ▼           ▼           ▼
 Relational   Semantic     Web
 Database     Retrieval   Application
      │           │           │
      ▼           ▼           ▼
 PostgreSQL    pgvector    Next.js
      │           │           │
      └───────────┼───────────┘
                  ▼
          Intelligent Coursework
              Retrieval
```

The project focuses on demonstrating how a relational database and vector similarity search can work together to solve a real academic information-retrieval problem.

---

## Contributors

| Contributor | Responsibilities |
|-------------|-------------------|
| TBD | Database / Backend / Application |
| TBD | Database / ML / Application |

*Both team members will contribute to both database and application development.*

---

## License

This project is developed for academic and educational purposes. License information will be finalized before the project release.

---

## Acknowledgements

StudyMate uses the following open-source technologies:

- [Next.js](https://nextjs.org/)
- [TypeScript](https://www.typescriptlang.org/)
- [PostgreSQL](https://www.postgresql.org/)
- [pgvector](https://github.com/pgvector/pgvector)
- [Prisma](https://www.prisma.io/)
- [Python](https://www.python.org/)
- [FastAPI](https://fastapi.tiangolo.com/)
- [Sentence Transformers](https://www.sbert.net/)
- [PyMuPDF](https://pymupdf.readthedocs.io/)
- [Tailwind CSS](https://tailwindcss.com/)
- [shadcn/ui](https://ui.shadcn.com/)

---

<div align="center">

# StudyMate

**Smart Notes. Semantic Search. Better Coursework Discovery.**

</div>
