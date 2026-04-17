# Story 1.1: Frontend and Backend Foundation

Status: ready-for-dev

## Story

As a developer,
I want a working React + FastAPI baseline wired through a shared API contract,
so that all later features build on a stable application foundation.

## Acceptance Criteria

1. Frontend app scaffold runs in container and serves core routes/pages.
2. Backend FastAPI scaffold runs in container with base API router mounting.
3. Frontend API client reaches backend through configured base URL.
4. A basic health or smoke route proves FE-BE communication end to end.

## Tasks / Subtasks

- [ ] Scaffold frontend shell and routing (AC: 1)
  - [ ] Set up page structure for chat, project, and settings screens.
  - [ ] Add shared API utility module for backend calls.
- [ ] Scaffold backend app and router registration (AC: 2)
  - [ ] Create FastAPI entrypoint and route module organization.
  - [ ] Add initial health/smoke endpoint.
- [ ] Wire FE-BE communication (AC: 3, 4)
  - [ ] Configure base URL and environment handling for frontend.
  - [ ] Validate request from frontend to backend endpoint.

## Dev Notes

- Follow architecture tree for module locations to avoid future refactors.
- Keep this story intentionally minimal: structure over full feature behavior.
- Ensure naming conventions are consistent between frontend and backend routes.

### Project Structure Notes

- Frontend focus: `frontend/src/pages`, `frontend/src/components`, `frontend/src/lib`.
- Backend focus: `backend/main.py`, `backend/api/*`.
- Keep route prefixing compatible with `/api/*` contract.

### References

- [Source: docs/epics.md#Epic 1 - Core Application Foundation]
- [Source: docs/architecture.md#2. Tech Stack]
- [Source: docs/architecture.md#3. Project Structure]
- [Source: docs/architecture.md#5. API Routes]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
