# Story 5.2: Project-Scoped RAG Retrieval

Status: ready-for-dev

## Story

As a user,
I want answers grounded only in my selected project's sources,
so that responses are relevant and do not leak across projects.

## Acceptance Criteria

1. Backend embeds user query and retrieves top chunks only from selected project collection.
2. Prompt assembly uses system instructions, retrieved chunks, and conversation window.
3. If no relevant context is found, assistant returns explicit no-result response.
4. System never fabricates citations when retrieval yields no relevant source.

## Tasks / Subtasks

- [ ] Implement query embedding + retrieval service flow (AC: 1)
  - [ ] Resolve active embedding model from current settings.
  - [ ] Query Chroma collection keyed by project ID.
- [ ] Implement prompt construction for generation (AC: 2)
  - [ ] Compose context blocks with chunk excerpts and metadata.
  - [ ] Include conversation window content passed from frontend.
- [ ] Implement no-result handling policy (AC: 3, 4)
  - [ ] Define confidence/empty-threshold behavior.
  - [ ] Return explicit fallback response without citations.

## Dev Notes

- Retrieval isolation is a product-level trust requirement.
- Keep retrieval API provider-agnostic and reusable for streaming story.
- Ensure retrieved metadata can back citation payloads later in pipeline.

### Project Structure Notes

- Backend modules: `backend/services/retrieval.py`, `backend/services/embeddings.py`, `backend/api/chat.py`.
- Chroma collection per project is a non-negotiable architecture decision.

### References

- [Source: docs/epics.md#Epic 5 - Chat Experience & RAG Pipeline]
- [Source: docs/prd.md#5.4 Chat]
- [Source: docs/architecture.md#6.2 RAG Query Pipeline]
- [Source: docs/architecture.md#10. Key Design Decisions]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
