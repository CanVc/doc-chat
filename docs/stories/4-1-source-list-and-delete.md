# Story 4.1: Source List and Delete

Status: ready-for-dev

## Story

As a user,
I want to view and remove sources within a project,
so that I can keep indexed knowledge accurate and up to date.

## Acceptance Criteria

1. Project page lists indexed sources with name, type, description, date added, and chunk count.
2. `GET /api/projects/{id}/sources` returns source records scoped to selected project.
3. User can delete a source from UI and backend via `DELETE /api/projects/{id}/sources/{source_id}`.
4. Deleting a source removes associated chunks from ChromaDB and updates UI list.

## Tasks / Subtasks

- [ ] Implement source list API and data schema (AC: 1, 2)
  - [ ] Store and return source metadata fields required by PRD.
  - [ ] Enforce project scoping in list queries.
- [ ] Implement source delete backend flow (AC: 3, 4)
  - [ ] Remove source metadata and vector chunks atomically where possible.
  - [ ] Return deterministic not-found and success responses.
- [ ] Implement source list and delete UI interactions (AC: 1, 3, 4)
  - [ ] Render metadata table/list in project context.
  - [ ] Support delete action with confirmation and loading states.

## Dev Notes

- Keep source metadata model aligned with architecture contract.
- Deletion must be project-safe to avoid cross-project data loss.
- UX should make destructive action clear and reversible only by re-ingestion.

### Project Structure Notes

- Backend: `backend/api/sources.py`, ingestion/source services, Chroma integration helpers.
- Frontend: `frontend/src/components/sources/*`, `frontend/src/pages/ProjectPage.tsx`.

### References

- [Source: docs/epics.md#Epic 4 - Source Management & Replacement Safety]
- [Source: docs/prd.md#4.5 User - Managing Sources]
- [Source: docs/prd.md#5.3 Source Management]
- [Source: docs/architecture.md#4.3 Source]
- [Source: docs/architecture.md#5. API Routes]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
