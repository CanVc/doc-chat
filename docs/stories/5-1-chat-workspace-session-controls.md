# Story 5.1: Chat Workspace Session Controls

Status: ready-for-dev

## Story

As a user,
I want clear conversation controls around project selection and reset,
so that each chat stays scoped and understandable.

## Acceptance Criteria

1. User selects a project before first message in a new conversation.
2. Project selector locks after first sent message for that conversation.
3. "New conversation" resets chat state and unlocks project selector.
4. Conversation state is session-only and clears on page reload.

## Tasks / Subtasks

- [ ] Implement chat session state model in frontend (AC: 1, 4)
  - [ ] Track selected project and message history in client state only.
  - [ ] Ensure reload clears conversation by design.
- [ ] Enforce project lock lifecycle (AC: 2)
  - [ ] Disable project selector after first successful send.
  - [ ] Prevent sending message without selected project.
- [ ] Implement new conversation reset flow (AC: 3)
  - [ ] Add reset control and state clearing behavior.
  - [ ] Re-enable selector immediately after reset.

## Dev Notes

- Keep chat UX intuitive for non-technical users.
- Scope isolation at interaction level prevents accidental cross-project context.
- Do not introduce persistent chat storage in v1.

### Project Structure Notes

- Frontend modules: `frontend/src/components/chat/*`, `frontend/src/pages/ChatPage.tsx`.
- API payloads should carry selected project ID on each send.

### References

- [Source: docs/epics.md#Epic 5 - Chat Experience & RAG Pipeline]
- [Source: docs/prd.md#4.4 User - Chatting with Documents]
- [Source: docs/prd.md#5.4 Chat]
- [Source: docs/architecture.md#6.3 Conversation Context Management]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
