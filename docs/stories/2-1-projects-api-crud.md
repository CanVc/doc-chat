# Story 2.1: Projects API CRUD

Status: ready-for-dev

## Story

As a user,
I want to create, list, rename, and delete projects through API routes,
so that I can organize document collections for different topics.

## Acceptance Criteria

1. `GET /api/projects` lists all projects with stable schema (`id`, `name`, optional `description`, timestamps).
2. `POST /api/projects` creates a project with required name and optional description.
3. `PATCH /api/projects/{id}` renames/updates a project and returns updated representation.
4. `DELETE /api/projects/{id}` deletes project metadata and signals successful removal.
5. Input validation and not-found responses are explicit and consistent.

## Tasks / Subtasks

- [ ] Implement project persistence model/index (AC: 1, 2)
  - [ ] Add storage format and helper functions for project records.
  - [ ] Enforce unique ID generation and name validation.
- [ ] Implement project API routes (AC: 1, 2, 3, 4)
  - [ ] Add route handlers in `backend/api/projects.py`.
  - [ ] Return typed responses with proper HTTP status codes.
- [ ] Add validation and error semantics (AC: 5)
  - [ ] Handle invalid payloads, missing projects, and empty names.

## Dev Notes

- Keep this story API-first; UI wiring is handled in a separate story.
- Prepare delete route to be extended with cascade cleanup logic.
- Ensure response contracts remain stable for frontend integration.

### Project Structure Notes

- Main implementation in `backend/api/projects.py` and related service modules.
- Persist lightweight project index alongside existing backend data storage.

### References

- [Source: docs/epics.md#Epic 2 - Project Lifecycle Management]
- [Source: docs/prd.md#5.1 Project Management]
- [Source: docs/architecture.md#4.2 Project]
- [Source: docs/architecture.md#5. API Routes]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
