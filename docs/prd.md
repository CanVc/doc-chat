# Product Requirements Document - DocChat

**Status:** Draft  
**Version:** 0.1  
**Last updated:** 2026-04-16

---

## 1. Overview

DocChat is a self-hosted web application that lets a team index internal documentation and chat with it using a RAG (Retrieval-Augmented Generation) pipeline. It is deployed via Docker Compose, requires no SaaS subscription, and keeps data on company-controlled infrastructure.

**Core value proposition:** turn a pile of documents into something you can have a conversation with - in minutes, on your own server.

> **Note on data privacy:** document storage and vector indexing run locally. When OpenAI is configured, document content is sent to OpenAI for embedding generation (ingestion time) and relevant excerpts are sent to the chat model (query time). For fully air-gapped usage, Ollama with a local embedding model is supported - no data leaves the machine.

---

## 2. Goals & Success Criteria

| Goal | Success Criteria |
|---|---|
| Fast onboarding | A non-technical user can upload a document and get a relevant answer in under 2 minutes |
| Simple deployment | The full stack runs with `docker compose up` on a fresh machine |
| Source transparency | Every chatbot answer based on retrieved content includes citations (document name + relevant excerpt) |
| Easy ingestion | Ingestion starts within 5 seconds of upload, with a live progress indicator throughout |

---

## 3. Users & Personas

**The Admin** - the person who deploys and maintains DocChat. Runs `docker compose up`, then configures everything (providers, API keys, models) directly from the Settings page in the app. Also manages projects and sources. No need to touch the `.env` file beyond initial infrastructure setup (ports, volume paths).

**The User** - a team member who asks questions. Does not need to understand how RAG works. Expects a chat interface that behaves like a smart search engine. Could be a developer, a manager, or a new hire.

> There is no authentication system in v1. Security is handled at the network level. Anyone on the network can use the app.

---

## 4. User Journeys

### 4.1 Admin - First Setup

1. Clones the repository
2. Runs `docker compose up`
3. Opens the app in a browser
4. Goes to Settings -> Providers
5. Selects OpenAI and enters the API key, or selects Ollama and enters the network URL (for example `http://192.168.1.x:11434`)
6. The app scans available models and displays them
7. Selects a chat model and an embedding model
8. Creates a first project ("Company Handbook")
9. Uploads a PDF and sees ingestion progress
10. Asks a test question and sees a cited answer
11. Shares the URL with the team

### 4.2 Admin - Changing Provider or Model

1. Opens Settings -> Providers
2. Updates the API key, switches provider, or selects a different model
3. Saves - change takes effect immediately, no restart required

### 4.3 User - Ingesting a New Document

1. Opens the app
2. Selects an existing project or creates a new one
3. Clicks "Add source"
4. Uploads a file (PDF, Word, Markdown, TXT, CSV) or pastes a URL
5. Optionally adds a description to help the LLM understand the document's context
6. If the file is already indexed (same content hash), the button reads "Replace" instead of "Add"
7. Sees live ingestion progress, then confirmation: "42 chunks indexed - source added"
8. Source appears in the project's source list

### 4.4 User - Chatting with Documents

1. Opens the chat and selects a project from the dropdown in the chat area
2. Types a question and sends it - the project selector is now locked for this conversation
3. Sees the answer stream token by token
4. Each answer based on retrieved content includes cited sources (document name + excerpt)
5. Continues the conversation - previous messages are kept as context
6. If the conversation grows long, a subtle notice appears: "Earlier messages have been removed from context"
7. Clicks "New conversation" to start fresh - project selector becomes available again
8. Closes the tab - conversation is not saved

### 4.5 User - Managing Sources

1. Opens a project
2. Views the list of indexed sources (name, file type, date added, chunk count)
3. Deletes a source - its chunks are removed from the vector store
4. Re-uploads an updated version of the same document

---

## 5. Functional Requirements

### 5.1 Project Management

- FR-01: User can create a project with a name and optional description
- FR-02: User can rename or delete a project
- FR-03: Deleting a project removes all associated sources and vector data
- FR-04: Projects are listed in the sidebar and persist across sessions

### 5.2 Document Ingestion

- FR-05: User can upload one or multiple files at once (max 50 MB per file)
- FR-06: Supported file formats: PDF, Word (.docx), Markdown (.md), plain text (.txt), CSV
- FR-07: User can ingest a single web page by pasting an `http` or `https` URL (single page only: no link following/crawling, no JavaScript rendering, no authenticated pages)
- FR-08: URL ingestion network limits are fixed to a 30-second timeout and a maximum of 5 redirects
- FR-09: Ingestion starts within 5 seconds of file upload or URL submission and displays a live progress indicator
- FR-10: After ingestion, the UI displays the number of chunks indexed and confirms the source name
- FR-11: Ingestion errors are surfaced clearly (unsupported format, file too large, unreachable URL, empty document, unsupported URL scheme, timeout, too many redirects, unsupported content type, page size above 50 MB)

### 5.3 Source Management

- FR-12: Each project displays a list of its indexed sources (name, type, description, date, chunk count)
- FR-13: User can delete a source - all associated chunks are removed from the vector store
- FR-14: The system detects duplicate sources by content hash - if the file is already indexed, the "Add" button becomes "Replace"
- FR-15: Replacing a source is atomic: the new version is fully ingested and validated before activation, then the system switches to it in one step and removes old chunks; if ingestion fails, the previous version remains active (rollback behavior)
- FR-16: User can add an optional description to a source at ingestion time - stored as metadata on all its chunks

### 5.4 Chat

- FR-17: On a fresh conversation, the user selects a project from a dropdown in the chat area
- FR-18: After the first message is sent, the project selector is locked for that conversation
- FR-19: User can start a new conversation at any time - project selector resets
- FR-20: The chatbot answers based exclusively on the selected project's documents
- FR-21: Responses stream token by token
- FR-22: Every response that uses retrieved project content includes at least one source citation (document name + short excerpt)
- FR-23: If no relevant content is found, the chatbot says so explicitly and does not fabricate citations
- FR-24: Conversation history is maintained within the session using a sliding window - oldest messages are dropped when the context limit is reached
- FR-25: When messages are dropped from context, a subtle notice is displayed in the chat: "Earlier messages have been removed from context"
- FR-26: Conversation history is not persisted - it resets on page reload

### 5.5 Administration

- FR-27: Admin can configure LLM providers from the Settings page - no API key required in the `.env` file
- FR-28: Supported providers: OpenAI (requires API key) and Ollama (requires network URL and port, no key)
- FR-29: For Ollama, admin must have both a chat model and an embedding model available on the Ollama server before configuring DocChat - the UI displays a reminder
- FR-30: On provider save, the app scans available models from the provider and displays them
- FR-31: Admin selects one chat model and one embedding model - applied globally to all projects
- FR-32: Changing the embedding model triggers a warning: all existing sources must be re-ingested for the change to take effect
- FR-33: The Settings page displays token consumption and estimated cost in USD per project and globally; OpenAI cost estimation is based on https://openai.com/fr-FR/api/pricing/
- FR-34: For Ollama, token consumption is displayed without cost estimation
- FR-35: The UI displays basic system status: number of projects, total sources, storage used
- FR-36: If a provider response does not include token usage for a request, the UI marks usage/cost as unavailable for that request and excludes it from estimated cost totals

---

## 6. Non-Functional Requirements

- NFR-01: The full stack is deployable with `docker compose up`. The `.env` file contains only infrastructure configuration (ports, volume paths) - no API keys. All provider configuration is done via the Settings page.
- NFR-02: Document storage and vector indexing run entirely on the host machine. Query-time document excerpts are sent to the configured LLM provider (OpenAI or Ollama). With Ollama, no data leaves the machine.
- NFR-03: Connectivity behavior is provider-dependent: OpenAI mode requires internet access for embedding and chat API calls, while Ollama mode can run fully offline/air-gapped when Ollama is local or on a private network.
- NFR-04: The UI must be usable on desktop browsers (Chrome, Firefox, Safari) - no mobile requirement for v1
- NFR-05: All code, comments, and documentation are written in English

---

## 7. Out of Scope - v1

| Feature | Reason |
|---|---|
| User authentication & permissions | Adds significant complexity; network-level security is sufficient for v1 |
| Persistent conversation history | Requires a database; session-only is acceptable for v1 |
| Cross-project search | Adds query complexity; per-project isolation is the intended UX |
| Excel (.xlsx) ingestion | Table-aware chunking is non-trivial; workaround: export to CSV |
| Multi-URL crawling / domain scraping | Out of scope; single URL ingestion covers the main need |
| Mobile-optimized UI | Nice to have, not critical for a portfolio project |
| Analytics / usage dashboard | Future version |
| Role-based access control | Future version |

---

## 8. Decisions

| # | Decision | Status |
|---|---|---|
| 1 | Ollama supports both chat and embedding models | Decided |
| 2 | URL ingestion is single page only | Decided |
| 3 | Maximum upload size is 50 MB per file | Decided |

---

## 9. Open Questions

| # | Question | Owner | Status |
|---|---|---|---|
| 1 | Should the Settings page be protected by a simple password? | - | Open |

---

*This document is the source of truth for DocChat v1 scope. Anything not listed here is out of scope unless explicitly added.*
