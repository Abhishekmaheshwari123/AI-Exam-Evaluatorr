AI-Exam-Evalutor
=======
# AI-Exam-Evaluator

Step 0 — What We Are Building

We are building an AI-based exam evaluation platform similar to Gradescope.

Goal:

Automatically evaluate answer sheets using:

    OCR (extract text)

    AI grading

    scalable backend

    distributed processing

Step 1 — System Requirements

In system design, the first step is always requirements.

Functional Requirements

Teacher should be able to:

Create an exam

    Upload answer key

    Upload answer sheets

    See evaluated results

    Student should be able to:

    View marks

    View feedback

    View evaluated answers

Non-Functional Requirements

These define how good the system should be.

Important ones:

    Scalability

    System should process thousands of answer sheets.

    Reliability

    System should not lose submissions.

    Performance

    Evaluation should happen asynchronously.

    Storage

    Large number of PDFs/images must be stored.


Step 2 — Identify Core Entities

Every system revolves around data entities.

Main entities in our system:

    Teacher
    Student
    Exam
    AnswerKey
    Submission
    Result
    AnswerSheet


Step 3 — System Workflow

Let’s understand the end-to-end flow.

                    Teacher Creates Exam
                            |
                            v
                    Teacher Uploads Answer Key
                            |
                            v
                    Teacher Uploads Answer Sheets
                            |
                            v
                    System Stores Files
                            |
                            v
                    OCR Extracts Text
                            |
                            v
                    AI Evaluates Answers
                            |
                            v
                    Marks + Feedback Generated
                            |
                            v
                    Results Stored in Database
                            |
                            v
                    Teacher / Student Dashboard

This is the core pipeline of the system.



Step 4 — First Architecture (Very Simple)

We always start with a simple architecture.

        +----------------------+
        |   Teacher / Student  |
        +----------+-----------+
                   |
                   v
        +----------------------+
        |       Frontend       |
        |       (React)        |
        +----------+-----------+
                   |
                   v
        +----------------------+
        |      Backend API     |
        |   (Node.js Server)   |
        +----------+-----------+
                   |
        +----------+-----------+
        |                      |
        v                      v
+---------------+     +----------------+
|   Database    |     |  File Storage  |
| PostgreSQL    |     | PDF / Images   |
+---------------+     +----------------+

At this stage the system only:

    stores exam data

    stores uploaded answer sheets

    No AI yet.

Step 5 — Add AI Processing Layer

Now we introduce OCR and AI grading.

    +-----------------------------+
    |Teacher Uploads Answer Sheets|
    +-----------------------------+
           |
           v
    +-------------+
    |   Backend   |
    +------+------+ 
           |
           v
    +-------------+
    | File Storage|
    +------+------+
           |
           v
    +-------------+
    | OCR Service |
    | Text Extract|
    +------+------+
           |
           v
    +-------------+
    | AI Grading  |
    | NLP Model   |
    +------+------+
           |
           v
    +-------------+
    |   Results   |
    |  Database   |
    +-------------+

OCR will be done using Tesseract OCR.

Step 6 — Problem With This Architecture

Imagine a teacher uploads 2000 answer sheets.

If backend processes them directly:

    Server becomes slow
    Requests block
    System crashes

    Solution → Asynchronous Processing



Step 7 — Introducing Queue System

Now we add a queue like Apache Kafka.

New architecture:

        Teacher Upload
            |
            v
        +-------------+
        |   Backend   |
        +------+------+
            |
            v
        +-------------+
        |  Job Queue  |
        |   (Kafka)   |
        +------+------+
            |
            v
        +-------------+
        | OCR Workers |
        +------+------+
            |
            v
        +-------------+
        | AI Workers  |
        +------+------+
            |
            v
        +-------------+
        |  Database   |
        +-------------+

Workers process answer sheets in parallel.



Step 8 — Adding Cache Layer

Frequent queries like:

    exam list

    results

    analytics

    should be cached using Redis.

Architecture becomes:

        Client
        |
        v
        Frontend
        |
        v
        Backend API
        |
        +-------> Redis Cache
        |
        v
        Database


Step 9 — Final High Level Architecture

                                +----------------------+
                                |      Frontend        |
                                |        React         |
                                +----------+-----------+
                                        |
                                        v
                                +----------------------+
                                |      Backend API     |
                                |      Node.js         |
                                +----+-----------+-----+
                                    |           |
                                    v           v
                            +---------+   +--------+
                            |  Redis  |   |Database|
                            |  Cache  |   |Postgres|
                            +----+----+   +----+---+
                                    |
                                    v
                            +----------+
                            | Job Queue|
                            |  Kafka   |
                            +----+-----+
                                    |
                    +-------------+-------------+
                    |                           |
                    v                           v
                +-----------+               +-----------+
                | OCR Worker|               | AI Worker |
                +-----------+               +-----------+
                    |                           |
                    +------------+--------------+
                                |
                                v
                            +-----------+
                            |  Results  |
                            +-----------+
Step 10 — Development Plan

We will build the system in this exact order.

Phase 1
Project structure


Phase 2
Database design


Phase 3
Backend APIs


Phase 4
Frontend upload system


Phase 5
OCR pipeline


Phase 6
AI grading engine


Phase 7
Queue workers


Phase 8
Caching


Phase 9
Scaling architecture