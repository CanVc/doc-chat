# Epics — DocChat

> Implementation order. Each epic delivers something tangible and testable before moving to the next.

---

## Epic 0 — Dev Environment Setup

Set up the development environment so every tool needed to build the project is installed, configured, and working locally.

**Done when:** a developer can clone the repo, run a single setup script, and have the full local environment ready to code.

---

## Epic 1 — Foundation

Scaffold the full project structure. A FastAPI backend that responds to requests, a React frontend that connects to it, both running in Docker Compose.

**Done when:** `docker compose up` starts both services, the frontend displays a placeholder page, and a `/api/health` endpoint returns `200 OK`.

---

## Epic 2 — Project Management

Create, rename, and delete projects. Projects are listed in the sidebar and persist across sessions.

**Done when:** a user can create multiple projects, navigate between them, and delete one — with all UI state reflecting the changes correctly.

---

## Epic 3 — Document Ingestion

Upload files (PDF, Word, Markdown, TXT, CSV) or paste a URL. Files are parsed, chunked, embedded, and stored in ChromaDB under the correct project collection. Sources are listed per project with their metadata.

**Done when:** a user can upload a PDF, see the ingestion progress, and confirm the number of chunks indexed. The source appears in the project source list with name, type, description, date, and chunk count.

---

## Epic 4 — Source Management

List, delete, and replace sources. Duplicate detection by content hash. Optional description at ingestion time stored as chunk metadata.

**Done when:** uploading a known file shows a "Replace" button instead of "Add". Deleting a source removes all its chunks from ChromaDB. Source descriptions are stored and visible in the list.

---

## Epic 5 — RAG & Chat

Full RAG pipeline: embed the question, retrieve top-k chunks, build prompt, call the LLM, stream the response. Responses include source citations. Conversation history managed with a sliding window.

**Done when:** a user can select a project, ask a question, and receive a streamed cited answer. Long conversations trigger the "Earlier messages have been removed from context" notice.

---

## Epic 6 — Settings & Providers

Configure LLM providers (OpenAI or Ollama) from the UI. Scan available models. Select chat and embedding models globally. API key stored server-side and displayed masked.

**Done when:** an admin can switch between OpenAI and Ollama, scan models, select one of each type, and have all subsequent chat and ingestion use the new configuration — without restarting the app.

---

## Epic 7 — Usage & Monitoring

Display token consumption and estimated cost per project and globally. Display system status (projects count, sources count, storage used). No cost estimation for Ollama.

**Done when:** the Settings page shows a usage dashboard with token counts and USD estimates for OpenAI, and token counts only for Ollama.

---

## Epic 8 — Polish

Error handling, edge cases, UI feedback, loading states, empty states, responsive layout, README.

**Done when:** the app handles all documented error cases gracefully, every async action has a loading indicator, and the README allows a developer to deploy DocChat from scratch without additional help.

---

*Each epic maps to one or more API routes and frontend components. Stories will be derived from these epics.*
