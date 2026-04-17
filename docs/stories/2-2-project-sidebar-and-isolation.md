# Story 2.2: Project Sidebar and Isolation Behaviors

Status: ready-for-dev

## Story

As a user,
I want to manage projects directly from the sidebar,
so that project CRUD actions are visible and persistent across sessions.

## Acceptance Criteria

1. Sidebar lists projects from `GET /api/projects` and reflects updates after create/rename/delete.
2. User can create project with name and optional description from the UI.
3. User can rename and delete projects via explicit controls and confirmations.
4. Deleting a project triggers project-scoped cleanup behavior and removes it from UI state.
5. Project list persists across reloads via backend-backed storage.

## Tasks / Subtasks

- [ ] Build sidebar project state management (AC: 1, 5)
  - [ ] Fetch and render projects on app load.
  - [ ] Sync local UI state with API mutations.
- [ ] Implement create/rename/delete interactions (AC: 2, 3)
  - [ ] Add modal or inline forms with validation feedback.
  - [ ] Add user confirmation for destructive delete action.
- [ ] Connect delete flow to isolation guarantees (AC: 4)
  - [ ] Ensure deleted project disappears immediately from navigation.
  - [ ] Handle API cleanup errors without stale UI state.

## Dev Notes

- Preserve a clean desktop-first UX with clear affordances.
- Keep optimistic updates safe; rollback on API failure.
- Ensure wording stays consistent with PRD user journeys.

### Project Structure Notes

- Frontend modules: `frontend/src/components/projects/*`, `frontend/src/pages/*`.
- API client hooks/utilities live under `frontend/src/hooks` or `frontend/src/lib`.

### References

- [Source: docs/epics.md#Epic 2 - Project Lifecycle Management]
- [Source: docs/prd.md#4.3 User - Ingesting a New Document]
- [Source: docs/prd.md#5.1 Project Management]
- [Source: docs/architecture.md#3. Project Structure]
- [Source: docs/architecture.md#5. API Routes]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
