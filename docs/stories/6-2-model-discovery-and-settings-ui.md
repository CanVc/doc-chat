# Story 6.2: Model Discovery and Settings UI

Status: ready-for-dev

## Story

As an admin,
I want to discover available provider models and select chat/embedding defaults,
so that DocChat uses valid runtime models for all projects.

## Acceptance Criteria

1. `GET /api/settings/providers/{provider}/models` scans and returns available models.
2. Settings UI supports provider switch, model scan, and selection of one chat model + one embedding model.
3. Ollama configuration flow shows reminder that both model types must exist server-side.
4. Changing embedding model shows warning that existing sources need re-ingestion.
5. Saving settings applies model choices globally without restart.

## Tasks / Subtasks

- [ ] Implement model discovery backend route (AC: 1)
  - [ ] Query provider adapters and normalize model response format.
  - [ ] Handle connectivity/provider errors with actionable messages.
- [ ] Build Settings provider/model UI workflow (AC: 2, 5)
  - [ ] Add provider selector, credentials/URL fields, and model dropdowns.
  - [ ] Persist selected chat and embedding models through settings API.
- [ ] Implement guided warnings and reminders (AC: 3, 4)
  - [ ] Display Ollama model availability reminder.
  - [ ] Display embedding-model-change re-ingestion warning before save.

## Dev Notes

- Keep provider abstraction the source of truth for model capabilities.
- UX must make global impact of model changes explicit.
- Immediate effect means no container restart assumptions.

### Project Structure Notes

- Backend: `backend/api/settings.py`, `backend/services/llm.py`, `backend/services/embeddings.py`.
- Frontend: `frontend/src/pages/SettingsPage.tsx`, `frontend/src/components/settings/*`.

### References

- [Source: docs/epics.md#Epic 6 - Provider & Model Administration]
- [Source: docs/prd.md#4.1 Admin - First Setup]
- [Source: docs/prd.md#5.5 Administration]
- [Source: docs/architecture.md#7. Provider Abstraction]
- [Source: docs/architecture.md#5. API Routes]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
