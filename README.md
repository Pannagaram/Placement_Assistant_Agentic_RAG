# Placement Assistant – Agentic RAG Chatbot

An AI-powered Placement Assistant built using Agentic RAG architecture to help students retrieve placement-related information, analyze resumes/job descriptions, and interact with placement data through a conversational chatbot.

The project uses:

- FastAPI backend
- LangGraph-based Agentic workflow
- ChromaDB vector database
- HuggingFace embeddings
- Gemini / Groq / Ollama LLM support
- React frontend
- Resume & JD analysis support

---

# Features

- Agentic RAG chatbot
- Fast and Agentic modes
- Semantic search using ChromaDB
- Resume analysis
- Job Description analysis
- Streaming token responses
- User authentication system
- Chat session saving/loading
- Multimodal document support
- Multiple LLM backend support
- Company-specific retrieval
- Aggregation-based queries

---

# Tech Stack

## Backend

- Python
- FastAPI
- LangChain
- LangGraph
- ChromaDB
- HuggingFace Embeddings
- Sentence Transformers

## Frontend

- React.js
- Node.js

## Embedding Model

```text
all-MiniLM-L6-v2
```

## Supported LLM Backends

- Gemini
- Groq
- Ollama

---

# System Architecture

```text
                    +----------------------+
                    |  Placement Documents |
                    | PDF / DOCX / PPTX    |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | generate_chunks.py   |
                    | Chunk Generation     |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | chunks_output.json   |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | store_embeddings.py  |
                    | Embedding Generation |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | ChromaDB Vector DB   |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | LangGraph Workflow   |
                    | Planner              |
                    | Retriever            |
                    | Executor             |
                    | Critic               |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | Gemini / Groq /      |
                    | Ollama LLM           |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | FastAPI Backend      |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    | React Frontend       |
                    +----------------------+
```

---

# Project Workflow

## 1. Chunk Generation

`generate_chunks.py` processes placement documents from:

- TXT
- DOCX
- PDF
- PPTX
- OCR-generated MMD files

It:

- extracts text
- formats markdown
- performs semantic chunking
- classifies sections
- generates metadata

The script classifies chunks into sections such as:

- Eligibility Criteria
- Compensation
- Job Description
- Selection Rounds
- Skill Requirements

---

## 2. Embedding Generation

`store_embeddings.py`:

- loads generated chunks
- generates embeddings using `all-MiniLM-L6-v2`
- stores vectors in ChromaDB

Each chunk stores metadata:

- company
- role
- section
- filename
- file type
- header

---

## 3. Vector Retrieval

The retriever:

- detects company names
- performs semantic retrieval
- supports aggregation queries
- supports metadata filtering

If a company is detected, local chunks are prioritized before semantic vector search.

---

## 4. Agentic RAG Workflow

The project uses LangGraph to build an Agentic workflow containing:

## Planner

Optimizes user queries before retrieval.

## Retriever

Fetches relevant chunks from ChromaDB or local chunk storage.

## Executor

Generates grounded responses using retrieved context.

Supports:

- Gemini REST streaming
- Groq streaming
- Ollama streaming

## Critic

Validates generated responses against retrieved context in Agentic mode.

---

# Fast Mode vs Agentic Mode

| Fast Mode | Agentic Mode |
|---|---|
| Single-pass execution | Multi-step reasoning |
| Faster responses | Higher response quality |
| No critique loop | Critic validation |
| Lower latency | More accurate grounding |

---

# API Features

The FastAPI backend provides:

## Authentication APIs

- Signup
- Login

Passwords are hashed using SHA256.

---

## Chat APIs

Streaming chatbot responses using async token queues.

---

## Session Management

Supports:

- save sessions
- load sessions
- delete sessions

The backend stores the last 10 sessions per user.

---

## Resume & JD Analysis

Supports uploads for:

- PDF
- DOCX
- PPTX
- Images

The system performs multimodal analysis using Gemini or Groq.

---

# Design Decisions

## Why ChromaDB?

Chosen because:

- lightweight local vector database
- easy LangChain integration
- simple setup
- suitable for academic projects

---

## Why MiniLM Embeddings?

`all-MiniLM-L6-v2` was selected because:

- fast embedding generation
- lightweight model
- low memory usage
- suitable for CPU inference

---

## Why Agentic Workflow?

The project supports:

- query planning
- validation
- iterative correction
- grounded response generation

using LangGraph nodes instead of a simple retrieval chain.

---

## Why Multiple LLM Backends?

The architecture supports:

- Gemini
- Groq
- Ollama

to provide flexibility between:

- cloud APIs
- local inference
- latency optimization

---

# Tradeoffs

| Decision | Benefit | Tradeoff |
|---|---|---|
| ChromaDB local storage | Simple setup | Limited scalability |
| MiniLM embeddings | Fast and lightweight | Lower accuracy than large embedding models |
| Agentic mode | Better grounding | Higher latency |
| Multiple backend support | Flexible deployment | Increased complexity |
| Streaming responses | Better UX | More async handling complexity |

---

# Supported Queries

Examples:

- What is the stipend for Amazon?
- What are the eligibility criteria for Bosch?
- List all companies for 2026
- Analyze my resume
- Compare company packages

---

# Folder Structure

```text
Placement_Assistant/
│
├── api.py
├── rag_agent.py
├── generate_chunks.py
├── store_embeddings.py
├── query_db.py
├── requirements.txt
├── chunks_output.json
├── chroma_db/
│
├── frontend/
│
├── Placements_Data/
├── OCR_mmd_Collected/
│
├── users.json
├── chat_sessions.json
│
└── README.md
```

---

# Setup Instructions

## Backend Setup

```bash
python -m venv venv
.\venv\Scripts\activate
pip install -r requirements.txt
```

---

## Environment Variables

Create `.env`

```env
GOOGLE_API_KEY=your_api_key
GROQ_API_KEY=your_api_key
LLM_BACKEND=gemini
AGENT_MODE=fast
```

---

## Generate Chunks

```bash
python generate_chunks.py
```

---

## Store Embeddings

```bash
python store_embeddings.py
```

---

## Run Backend

```bash
python api.py
```

Backend runs on:

```text
http://localhost:8000
```

---

## Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

---

# Future Improvements

Possible enhancements:

- cloud vector database deployment
- reranking pipeline
- hybrid search
- Redis caching
- JWT authentication
- scalable session storage
- production deployment pipeline

---

# Repository

Repository Link:

https://github.com/Pannagaram/Placement_Assistant_Agentic_RAG
