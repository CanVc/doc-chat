# Story 1.2: Persistence and Vector Primitives

Status: ready-for-dev

## Story

As a developer,
I want persistent settings/uploads storage and project-level Chroma collection utilities,
so that configuration and vector isolation survive container restarts.

## Acceptance Criteria

1. Backend can read/write `settings.json` in persistent data volume.
2. Uploaded source files are persisted under backend-managed storage paths.
3. Backend service layer can create and delete Chroma collections by project ID.
4. Collection lifecycle utilities are reusable by future project/source stories.

## Tasks / Subtasks

- [ ] Implement settings persistence service (AC: 1)
  - [ ] Create typed config read/write helpers with sane defaults.
  - [ ] Handle missing file initialization gracefully.
- [ ] Implement upload storage primitives (AC: 2)
  - [ ] Create backend-owned data directories.
  - [ ] Ensure UUID-based file naming scheme foundation.
- [ ] Implement Chroma collection management service (AC: 3, 4)
  - [ ] Add create/get/delete collection helpers.
  - [ ] Add basic error handling for idempotent operations.

## Dev Notes

- Persist only what architecture prescribes; avoid introducing a relational DB.
- Keep provider config and uploads on host-mounted volume.
- Chroma collection naming must map 1:1 with project isolation model.

### Project Structure Notes

- Backend files: `backend/config.py`, `backend/services/*`, `backend/data/*`.
- Volume mount expectations come from Compose (`/app/data` and Chroma volume).

### References

- [Source: docs/epics.md#Epic 1 - Core Application Foundation]
- [Source: docs/prd.md#6. Non-Functional Requirements]
- [Source: docs/architecture.md#3. Project Structure]
- [Source: docs/architecture.md#4. Data Model]
- [Source: docs/architecture.md#10. Key Design Decisions]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
