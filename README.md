AI-Exam-Evalutor
=======
# AI-Exam-Evaluator

🚀 Step 0 — What We Are Building

We are building an AI-based exam evaluation platform similar to Gradescope.

🎯 Goal

Automatically evaluate answer sheets using:

📄 OCR → extract text from answer sheets

🤖 AI grading → evaluate answers intelligently

⚡ Scalable backend → handle thousands of sheets

🔁 Distributed processing → process evaluations in parallel

📋 Step 1 — System Requirements

In system design, the first step is always defining requirements.

✅ Functional Requirements
👩‍🏫 Teacher Capabilities

Teachers should be able to:

Create an Exam

Upload Answer Key

Upload Answer Sheets

View Evaluated Results

🎓 Student Capabilities

Students should be able to:

View Marks

View Feedback

View Evaluated Answers

⚙️ Non-Functional Requirements

These define how well the system performs.

Requirement	Description
📈 Scalability	System should process thousands of answer sheets
🛡 Reliability	System should not lose submissions
⚡ Performance	Evaluation should run asynchronously
💾 Storage	Large number of PDFs / Images must be stored
🧩 Step 2 — Identify Core Entities

Every system revolves around data entities.

Main Entitiesin our system:

    Teacher
    Student
    Exam
    AnswerKey
    Submission
    Result
    AnswerSheet


🔄 Step 3 — System Workflow

Let’s understand the end-to-end flow of the system.

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


🏗 Step 4 — First Architecture (Simple Version)

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

✔ stores exam data
✔ stores uploaded answer sheets

No AI yet.

🤖 Step 5 — Adding AI Processing Layer

Now we introduce OCR + AI grading.

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

⚠️ Step 6 — Problem With This Architecture

Imagine a teacher uploads 2000 answer sheets.

If backend processes them directly:

❌ Server becomes slow
❌ Requests block
❌ System crashes

    Solution → Asynchronous Processing



🧠 Step 7 — Introducing Queue System

Solution → Asynchronous Processing

We introduce a message queue like Apache Kafka.


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



⚡ Step 8 — Adding Cache Layer

Some queries happen very frequently:

    exam list

    results

    analytics

    dashboard data

To reduce database load we add Redis Cache.

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

Benefits:

✔ Faster responses
✔ Reduced database load
✔ Better scalability



🧠 Step 9 — Final High Level Architecture

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

This architecture supports:

✔ high scalability
✔ parallel processing
✔ fault tolerance



🛠 STEP 10 — DEVELOPMENT PLAN

We will build the system step-by-step.



Phase 1  →  Project Structure

Phase 2  →  Database Design

Phase 3  →  Backend APIs

Phase 4  →  Frontend Upload System

Phase 5  →  OCR Pipeline

Phase 6  →  AI Grading Engine

Phase 7  →  Queue Workers

Phase 8  →  Caching

Phase 9  →  Scaling Architecture




💡 TECH STACK



Frontend

React  
TailwindCSS



Backend

Node.js  
Express



AI / Machine Learning

Python  
NLP Models  
Sentence Transformers



OCR

Tesseract OCR



Database

PostgreSQL



Queue System

Apache Kafka



Cache

Redis