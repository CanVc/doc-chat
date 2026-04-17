# Story 6.1: Provider Settings API and Secret Handling

Status: ready-for-dev

## Story

As an admin,
I want to configure OpenAI or Ollama providers in Settings without restarts,
so that model and connectivity changes apply immediately and securely.

## Acceptance Criteria

1. `GET /api/settings` returns current provider configuration with masked API key display value.
2. `PUT /api/settings/provider` saves provider config server-side and applies it immediately.
3. API key is never returned in clear text by any settings response.
4. Provider mode supports OpenAI (API key) and Ollama (URL/port) according to schema rules.
5. `.env` remains free of provider secrets.

## Tasks / Subtasks

- [ ] Implement settings read/write API contract (AC: 1, 2)
  - [ ] Add request/response models for provider settings.
  - [ ] Persist settings in backend data storage.
- [ ] Implement secure API key handling (AC: 3)
  - [ ] Store raw key server-side only.
  - [ ] Return masked surrogate field for display.
- [ ] Implement provider validation logic (AC: 4, 5)
  - [ ] Enforce required fields by provider type.
  - [ ] Keep separation between runtime provider config and infra `.env`.

## Dev Notes

- Settings are global across all projects in v1.
- Security posture requires masking and least exposure of secrets.
- Changes must be hot-applied without process restart.

### Project Structure Notes

- Backend modules: `backend/api/settings.py`, `backend/config.py`, provider abstraction modules.
- Frontend will consume these routes in subsequent settings UI story.

### References

- [Source: docs/epics.md#Epic 6 - Provider & Model Administration]
- [Source: docs/prd.md#5.5 Administration]
- [Source: docs/prd.md#6. Non-Functional Requirements]
- [Source: docs/architecture.md#4.1 settings.json]
- [Source: docs/architecture.md#5. API Routes]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
