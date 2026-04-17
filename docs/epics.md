# Epics - DocChat

> Implementation order for DocChat v1. Each epic delivers a testable increment aligned with `prd.md` and `architecture.md`.

---

## Epic 0 - Local Platform & Developer Setup

Establish a reproducible local environment for development and deployment with Docker Compose.

**FR/NFR coverage:**
- `NFR-01` - The full stack is deployable with `docker compose up`. The `.env` file contains only infrastructure configuration (ports, volume paths) - no API keys. All provider configuration is done via the Settings page.

**Done when:**
- A developer can clone the repository and run `docker compose up` on a fresh machine.
- The stack boots with three services: frontend, backend, and ChromaDB.
- Developer workstation prerequisites are documented and validated (`Docker` + Compose v2, `Python 3.11`, `venv`, and `Node.js`/`npm`).
- Backend local development setup is documented with `python -m venv` creation and dependency installation steps.
- Frontend local development setup is documented with Node package installation and dev run commands.
- `.env` is limited to infrastructure settings (ports/paths) and contains no API secrets.

---

## Epic 1 - Core Application Foundation

Scaffold frontend and backend structure, persistence primitives, and baseline API wiring.

**FR/NFR coverage:**
- `NFR-02` - Document storage and vector indexing run entirely on the host machine. Query-time document excerpts are sent to the configured LLM provider (OpenAI or Ollama). With Ollama, no data leaves the machine.

**Done when:**
- The React app and FastAPI app communicate through the configured API base URL.
- Backend persistence for `settings.json` and uploaded files is available through Docker volumes.
- Project-level ChromaDB collections can be created and deleted from backend services.

---

## Epic 2 - Project Lifecycle Management

Implement project CRUD and project-scoped isolation for sources and vectors.

**FR/NFR coverage:**
- `FR-01` - User can create a project with a name and optional description
- `FR-02` - User can rename or delete a project
- `FR-03` - Deleting a project removes all associated sources and vector data
- `FR-04` - Projects are listed in the sidebar and persist across sessions

**Done when:**
- `GET/POST/PATCH/DELETE /api/projects` are functional and reflected in the UI sidebar.
- Users can create projects with name and optional description, rename them, and delete them.
- Deleting a project removes associated sources and vector data (project isolation guaranteed).

---

## Epic 3 - Ingestion Jobs (Files + URL)

Implement ingestion as asynchronous jobs with progress streaming and strict validation rules.

**FR/NFR coverage:**
- `FR-05` - User can upload one or multiple files at once (max 50 MB per file)
- `FR-06` - Supported file formats: PDF, Word (.docx), Markdown (.md), plain text (.txt), CSV
- `FR-07` - User can ingest a single web page by pasting an `http` or `https` URL (single page only: no link following/crawling, no JavaScript rendering, no authenticated pages)
- `FR-08` - URL ingestion network limits are fixed to a 30-second timeout and a maximum of 5 redirects
- `FR-09` - Ingestion starts within 5 seconds of file upload or URL submission and displays a live progress indicator
- `FR-10` - After ingestion, the UI displays the number of chunks indexed and confirms the source name
- `FR-11` - Ingestion errors are surfaced clearly (unsupported format, file too large, unreachable URL, empty document, unsupported URL scheme, timeout, too many redirects, unsupported content type, page size above 50 MB)

**Done when:**
- `POST /api/projects/{id}/sources` accepts multi-file upload (PDF, DOCX, MD, TXT, CSV) and single-page URL ingestion.
- File ingestion enforces v1 limits: maximum `50 MB` per file.
- URL ingestion enforces v1 limits: `http/https` only, timeout `30s`, max redirects `5`, page size max `50 MB`.
- URL ingestion scope is restricted to one page (no crawling, no JavaScript rendering, no authenticated pages).
- Ingestion progress is streamed via `GET /api/projects/{id}/sources/jobs/{job_id}/events` with states such as `queued`, `parsing`, `chunking`, `embedding`, `storing`, `completed`, `failed`.
- Ingestion starts within 5 seconds of upload/submission and provides a live progress indicator.
- Successful completion returns chunk count and source name; documented errors are surfaced clearly in UI.

---

## Epic 4 - Source Management & Replacement Safety

Provide full source lifecycle with duplicate detection and safe replacement behavior.

**FR/NFR coverage:**
- `FR-12` - Each project displays a list of its indexed sources (name, type, description, date, chunk count)
- `FR-13` - User can delete a source - all associated chunks are removed from the vector store
- `FR-14` - The system detects duplicate sources by content hash - if the file is already indexed, the "Add" button becomes "Replace"
- `FR-15` - Replacing a source is atomic: the new version is fully ingested and validated before activation, then the system switches to it in one step and removes old chunks; if ingestion fails, the previous version remains active (rollback behavior)
- `FR-16` - User can add an optional description to a source at ingestion time - stored as metadata on all its chunks

**Done when:**
- Source list shows name, type, description, date, and chunk count per project.
- Duplicate detection by `content_hash` exposes a replace flow ("Add" -> "Replace").
- Replacement is atomic: old chunks stay active unless new ingestion fully succeeds (rollback on failure).
- Optional source description entered at ingestion is stored as metadata on all related chunks.
- Deleting a source removes all associated chunks from ChromaDB.

---

## Epic 5 - Chat Experience & RAG Pipeline

Deliver project-scoped chat with streaming responses, citations, and sliding-window context management.

**FR/NFR coverage:**
- `FR-17` - On a fresh conversation, the user selects a project from a dropdown in the chat area
- `FR-18` - After the first message is sent, the project selector is locked for that conversation
- `FR-19` - User can start a new conversation at any time - project selector resets
- `FR-20` - The chatbot answers based exclusively on the selected project's documents
- `FR-21` - Responses stream token by token
- `FR-22` - Every response that uses retrieved project content includes at least one source citation (document name + short excerpt)
- `FR-23` - If no relevant content is found, the chatbot says so explicitly and does not fabricate citations
- `FR-24` - Conversation history is maintained within the session using a sliding window - oldest messages are dropped when the context limit is reached
- `FR-25` - When messages are dropped from context, a subtle notice is displayed in the chat: "Earlier messages have been removed from context"
- `FR-26` - Conversation history is not persisted - it resets on page reload

**Done when:**
- User selects a project for a new conversation; selector locks after first message and unlocks on "New conversation".
- `POST /api/chat` streams SSE events (`start`, `token`, `citation`, `usage?`, `done`, `error`).
- Responses use only the selected project context and include citations (document name + excerpt) when retrieved content is used.
- If no relevant content is found, the assistant states it explicitly and does not fabricate citations.
- Conversation history is session-only (not persisted) with context truncation notice when applicable.

---

## Epic 6 - Provider & Model Administration

Enable runtime provider configuration from Settings with OpenAI/Ollama support.

**FR/NFR coverage:**
- `FR-27` - Admin can configure LLM providers from the Settings page - no API key required in the `.env` file
- `FR-28` - Supported providers: OpenAI (requires API key) and Ollama (requires network URL and port, no key)
- `FR-29` - For Ollama, admin must have both a chat model and an embedding model available on the Ollama server before configuring DocChat - the UI displays a reminder
- `FR-30` - On provider save, the app scans available models from the provider and displays them
- `FR-31` - Admin selects one chat model and one embedding model - applied globally to all projects
- `FR-32` - Changing the embedding model triggers a warning: all existing sources must be re-ingested for the change to take effect
- `NFR-03` - Connectivity behavior is provider-dependent: OpenAI mode requires internet access for embedding and chat API calls, while Ollama mode can run fully offline/air-gapped when Ollama is local or on a private network.

**Done when:**
- `GET /api/settings`, `PUT /api/settings/provider`, and model scan route are functional.
- Admin can configure OpenAI API key or Ollama URL from the UI with immediate effect (no restart).
- API key is stored server-side and only exposed as masked display value in API responses.
- Admin selects one chat model and one embedding model globally; Ollama reminder is displayed when required.
- Changing embedding model displays the re-ingestion warning for existing sources.

---

## Epic 7 - Usage & System Status

Expose observability data for usage and platform health in Settings.

**FR/NFR coverage:**
- `FR-33` - The Settings page displays token consumption and estimated cost in USD per project and globally; OpenAI cost estimation is based on https://openai.com/fr-FR/api/pricing/
- `FR-34` - For Ollama, token consumption is displayed without cost estimation
- `FR-35` - The UI displays basic system status: number of projects, total sources, storage used
- `FR-36` - If a provider response does not include token usage for a request, the UI marks usage/cost as unavailable for that request and excludes it from estimated cost totals

**Done when:**
- `GET /api/settings/usage` reports token consumption per project and globally.
- OpenAI usage includes estimated USD cost based on configured pricing rules.
- Ollama usage displays token counts without cost estimation.
- Missing provider usage data is marked unavailable and excluded from estimated totals.
- `GET /api/settings/status` returns projects count, total sources, and storage used.

---

## Epic 8 - Security, Reliability & UX Polish

Harden v1 behavior and finalize product quality for handoff.

**FR/NFR coverage:**
- `NFR-04` - The UI must be usable on desktop browsers (Chrome, Firefox, Safari) - no mobile requirement for v1
- `NFR-05` - All code, comments, and documentation are written in English

**Done when:**
- File ingestion validates extension, MIME type, and file signature before parsing.
- Upload paths are backend-generated and protected against path traversal under `data/uploads/`.
- CORS is restricted to explicit frontend origins (no wildcard).
- All documented async flows have clear loading/error/empty states and desktop browser usability.
- README documents setup and operations consistent with PRD scope and out-of-scope constraints.

---

*Out of scope remains unchanged from the PRD (no auth/roles, no persistent chat history, no cross-project search, no multi-URL crawling, no XLSX ingestion, no mobile-first requirement).* 
