# Story 7.2: Settings Observability Dashboard

Status: ready-for-dev

## Story

As an admin,
I want Settings to display usage and platform status metrics,
so that I can quickly assess system health and data footprint.

## Acceptance Criteria

1. `GET /api/settings/status` returns project count, total sources, and storage used.
2. Settings UI shows usage and status panels with per-project and global views.
3. OpenAI and Ollama views handle provider-specific cost behavior correctly.
4. Missing usage/cost data is clearly labeled unavailable in UI.

## Tasks / Subtasks

- [ ] Implement status endpoint backend aggregation (AC: 1)
  - [ ] Count projects and sources from persisted metadata.
  - [ ] Compute used storage from backend-managed files.
- [ ] Build Settings observability components (AC: 2, 3)
  - [ ] Render global and project-level usage cards/tables.
  - [ ] Render system status metrics section.
- [ ] Implement unavailable data presentation rules (AC: 4)
  - [ ] Display explicit unavailable markers.
  - [ ] Avoid misleading totals when data is missing.

## Dev Notes

- Keep panels lightweight and readable for non-technical admins.
- Ensure values refresh after provider/model changes and new chats.
- Avoid introducing advanced analytics beyond PRD scope.

### Project Structure Notes

- Backend: `backend/api/settings.py`, usage/status services.
- Frontend: `frontend/src/pages/SettingsPage.tsx`, `frontend/src/components/settings/*`.

### References

- [Source: docs/epics.md#Epic 7 - Usage & System Status]
- [Source: docs/prd.md#5.5 Administration]
- [Source: docs/architecture.md#5. API Routes]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
