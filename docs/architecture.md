# Architecture Document — DocChat

**Status:** Draft  
**Version:** 0.1  
**Last updated:** 2026-04-16

---

## 1. Overview

DocChat is a multi-container application orchestrated with Docker Compose. It consists of three services: a React frontend, a FastAPI backend, and a ChromaDB vector store. Configuration and uploaded files are persisted via Docker volumes.

```
┌─────────────────────────────────────────────────────┐
│                    Docker Network                    │
│                                                      │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────┐ │
│  │   Frontend   │   │   Backend    │   │ ChromaDB │ │
│  │ React + Vite │──▶│   FastAPI    │──▶│          │ │
│  │  Port 3000   │   │  Port 8000   │   │ Port 8001│ │
│  └──────────────┘   └──────┬───────┘   └──────────┘ │
│                            │                         │
└────────────────────────────┼────────────────────────┘
                             │
                    ┌────────┴────────┐
                    │  External APIs  │
                    │ OpenAI / Ollama │
                    └─────────────────┘
```

---

## 2. Tech Stack

| Layer | Technology | Reason |
|---|---|---|
| Frontend | React 18 + Vite | Fast dev experience, ecosystem |
| UI Components | shadcn/ui + TailwindCSS | Accessible, unstyled base + utility classes |
| Backend API | FastAPI + Python 3.11 | Async-native, auto-docs, type validation |
| Data validation | Pydantic v2 | Integrated with FastAPI, strict typing |
| Vector store | ChromaDB | Simple, local, persistent, no infra overhead |
| File parsing | pdfplumber, python-docx, built-in | Per format, battle-tested |
| HTTP client | httpx | Async, used for URL ingestion and LLM calls |
| LLM providers | OpenAI API, Ollama | Primary + local fallback |
| Config storage | JSON file on disk | Simple, no DB dependency for settings |
| File storage | Local filesystem | Uploaded sources kept after ingestion |
| Container | Docker + Docker Compose | Single-command deployment |

---

## 3. Project Structure

```
docchat/
├── docker-compose.yml
├── .env.example                  # ports, volume paths only — no secrets
│
├── frontend/
│   ├── Dockerfile
│   ├── src/
│   │   ├── components/
│   │   │   ├── chat/
│   │   │   ├── sources/
│   │   │   ├── projects/
│   │   │   └── settings/
│   │   ├── pages/
│   │   │   ├── ChatPage.tsx
│   │   │   ├── ProjectPage.tsx
│   │   │   └── SettingsPage.tsx
│   │   ├── hooks/
│   │   ├── lib/
│   │   └── main.tsx
│   └── package.json
│
└── backend/
    ├── Dockerfile
    ├── requirements.txt
    ├── main.py                   # FastAPI app entrypoint
    ├── config.py                 # settings.json read/write
    ├── api/
    │   ├── projects.py           # CRUD projects
    │   ├── sources.py            # ingestion, list, delete
    │   ├── chat.py               # RAG + streaming
    │   └── settings.py           # provider config, model selection
    ├── services/
    │   ├── ingestion.py          # orchestrates parsing → chunking → embedding → store
    │   ├── retrieval.py          # vector search
    │   ├── llm.py                # LLM provider abstraction (OpenAI / Ollama)
    │   ├── embeddings.py         # embedding provider abstraction
    │   └── parsers/
    │       ├── pdf.py
    │       ├── docx.py
    │       ├── text.py
    │       ├── csv.py
    │       └── url.py
    └── data/
        ├── settings.json         # persisted via Docker volume
        └── uploads/              # persisted via Docker volume
```

---

## 4. Data Model

### 4.1 settings.json

```json
{
  "provider": "openai",
  "openai_api_key": "sk-proj-ABC...XYZ",
  "openai_api_key_display": "sk-proj-ABC...XYZ",
  "ollama_url": null,
  "chat_model": "gpt-4o-mini",
  "embedding_model": "text-embedding-3-small"
}
```

> The API key is stored as-is server-side. Only the masked version (`sk-proj-ABC...XYZ`) is ever sent to the frontend.

### 4.2 Project

Stored as ChromaDB collection metadata + a lightweight JSON index.

```json
{
  "id": "uuid",
  "name": "Company Handbook",
  "description": "HR and internal policies",
  "created_at": "2026-04-16T10:00:00Z"
}
```

### 4.3 Source

```json
{
  "id": "uuid",
  "project_id": "uuid",
  "name": "onboarding_2024.pdf",
  "type": "pdf",
  "description": "Employee onboarding guide for new hires",
  "content_hash": "sha256:abc123...",
  "chunk_count": 42,
  "file_path": "data/uploads/uuid.pdf",
  "created_at": "2026-04-16T10:00:00Z"
}
```

### 4.4 ChromaDB Chunk (metadata per chunk)

```json
{
  "chunk_id": "uuid",
  "source_id": "uuid",
  "project_id": "uuid",
  "source_name": "onboarding_2024.pdf",
  "source_description": "Employee onboarding guide for new hires",
  "page": 3
}
```

---

## 5. API Routes

### Projects
| Method | Route | Description |
|---|---|---|
| GET | `/api/projects` | List all projects |
| POST | `/api/projects` | Create a project |
| PATCH | `/api/projects/{id}` | Rename a project |
| DELETE | `/api/projects/{id}` | Delete project + all its data |

### Sources
| Method | Route | Description |
|---|---|---|
| GET | `/api/projects/{id}/sources` | List sources for a project |
| POST | `/api/projects/{id}/sources` | Upload file or submit URL |
| DELETE | `/api/projects/{id}/sources/{source_id}` | Delete a source |

### Chat
| Method | Route | Description |
|---|---|---|
| POST | `/api/chat` | Send a message, returns SSE stream |

### Settings
| Method | Route | Description |
|---|---|---|
| GET | `/api/settings` | Get current config (masked API key) |
| PUT | `/api/settings/provider` | Save provider config |
| GET | `/api/settings/providers/{provider}/models` | Scan and return available models for a given provider |
| GET | `/api/settings/status` | Projects count, sources count, storage used |
| GET | `/api/settings/usage` | Token consumption + estimated cost |

---

## 6. Key Flows

### 6.1 Ingestion Pipeline

```
Upload (file or URL)
       │
       ▼
Compute content hash
       │
       ├─ Hash already in project? → Return { action: "replace_available" }
       │
       ▼
Parse text content
(pdfplumber / python-docx / plain text / httpx for URL)
       │
       ▼
Split into chunks
(recursive strategy, ~500 tokens, 50-token overlap)
       │
       ▼
Generate embeddings
(OpenAI API or Ollama — via embeddings.py abstraction)
       │
       ▼
Store in ChromaDB
(collection = project_id, metadata per chunk)
       │
       ▼
Save source record to project index
       │
       ▼
Return { chunk_count, source_name }
```

### 6.2 RAG Query Pipeline

```
User message
       │
       ▼
Embed the question
(same provider as ingestion)
       │
       ▼
Vector search in ChromaDB
(collection = project_id, top-k = 5)
       │
       ▼
Build prompt
(system prompt + retrieved chunks + conversation history)
       │
       ▼
Call LLM (streaming)
(OpenAI or Ollama — via llm.py abstraction)
       │
       ▼
Stream response via SSE
(token by token to frontend)
```

### 6.3 Conversation Context Management

- Conversation history is kept in frontend state (not persisted)
- On each message, the full history is sent to the backend in the request body
- Backend applies a sliding window: if total tokens (history + chunks + question) exceed the model's context limit, oldest messages are dropped
- When messages are dropped, the backend includes a flag in the response: `context_truncated: true`
- Frontend displays a subtle notice when this flag is received

---

## 7. Provider Abstraction

Both `llm.py` and `embeddings.py` expose a unified interface so the rest of the codebase is provider-agnostic.

```python
# llm.py
async def stream_chat(messages: list, model: str) -> AsyncIterator[str]:
    if provider == "openai":
        # call OpenAI chat completions API
    elif provider == "ollama":
        # call Ollama /api/chat

# embeddings.py
async def embed(texts: list[str], model: str) -> list[list[float]]:
    if provider == "openai":
        # call OpenAI embeddings API
    elif provider == "ollama":
        # call Ollama /api/embeddings
```

---

## 8. Docker Compose

```yaml
services:
  frontend:
    build: ./frontend
    ports:
      - "${FRONTEND_PORT:-3000}:3000"
    depends_on:
      - backend

  backend:
    build: ./backend
    ports:
      - "${BACKEND_PORT:-8000}:8000"
    volumes:
      - docchat_data:/app/data
    depends_on:
      - chromadb

  chromadb:
    image: chromadb/chroma:latest
    ports:
      - "8001:8001"
    volumes:
      - chroma_data:/chroma/chroma

volumes:
  docchat_data:    # settings.json + uploaded files
  chroma_data:     # vector store
```

> `.env` contains only `FRONTEND_PORT` and `BACKEND_PORT`. No secrets.

---

## 9. Key Design Decisions

| Decision | Choice | Reason |
|---|---|---|
| One ChromaDB collection per project | ✅ | Clean isolation, simple to delete, no cross-project bleed |
| Config stored as JSON | ✅ | Zero dependency, sufficient for a single-node deployment |
| Files kept after ingestion | ✅ | Allows re-ingestion if embedding model changes |
| History managed client-side | ✅ | No persistence requirement, simplifies backend |
| Sliding window for context | ✅ | Simple, predictable, honest to the user |
| Provider abstraction layer | ✅ | OpenAI and Ollama swappable without touching RAG logic |

---

*This document describes the v1 architecture. It is intended to evolve alongside the PRD.*
