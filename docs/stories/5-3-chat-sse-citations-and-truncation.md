# Story 5.3: Chat SSE, Citations, and Context Truncation Signals

Status: ready-for-dev

## Story

As a user,
I want streaming answers with transparent citations and context-limit notices,
so that I can trust responses and understand conversation constraints.

## Acceptance Criteria

1. `POST /api/chat` streams SSE events matching contract (`start`, `token`, `citation`, `usage?`, `error`, `done`).
2. When retrieved content is used, at least one citation event includes document name and excerpt.
3. Backend emits `context_truncated` flag on `start` when sliding window drops older messages.
4. Frontend shows subtle truncation notice when `context_truncated` is true.
5. Usage event includes token counts and nullable estimated cost field.

## Tasks / Subtasks

- [ ] Implement chat SSE response stream (AC: 1)
  - [ ] Emit events in expected order for normal and error flows.
  - [ ] Close stream cleanly on completion or failure.
- [ ] Implement citation and usage event emission (AC: 2, 5)
  - [ ] Map retrieved chunks to citation payload contract.
  - [ ] Emit usage fields with nullable cost support.
- [ ] Implement truncation signaling and UI notice (AC: 3, 4)
  - [ ] Apply sliding-window token budget logic.
  - [ ] Render contextual notification in chat UI.

## Dev Notes

- SSE contract fidelity is critical for frontend stability and observability.
- Maintain explicit behavior for request validation errors (HTTP 4xx, no stream).
- Keep truncation notice subtle but visible.

### Project Structure Notes

- Backend: `backend/api/chat.py`, `backend/services/llm.py`, context-window helpers.
- Frontend: chat stream consumer components and state reducers.

### References

- [Source: docs/epics.md#Epic 5 - Chat Experience & RAG Pipeline]
- [Source: docs/prd.md#5.4 Chat]
- [Source: docs/architecture.md#5. API Routes]
- [Source: docs/architecture.md#Chat SSE Contract (POST /api/chat)]
- [Source: docs/architecture.md#6.3 Conversation Context Management]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
