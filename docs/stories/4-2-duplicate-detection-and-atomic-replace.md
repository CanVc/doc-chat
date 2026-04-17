# Story 4.2: Duplicate Detection and Atomic Replace

Status: ready-for-dev

## Story

As a user,
I want duplicate detection and safe source replacement,
so that updating a document never leaves the project in a broken state.

## Acceptance Criteria

1. System computes `content_hash` and detects duplicates within a project.
2. UI exposes replace flow when duplicate is detected (`Add` -> `Replace`).
3. Replace operation is atomic: new source activates only after full ingestion + validation.
4. On replacement failure, previous source and chunks remain active (rollback behavior).
5. Optional source description is stored on source and propagated to all chunk metadata.

## Tasks / Subtasks

- [ ] Implement content hash duplicate detection (AC: 1, 2)
  - [ ] Compute deterministic hash from normalized source content.
  - [ ] Return duplicate signal through ingestion/job responses.
- [ ] Implement atomic replace transaction flow (AC: 3, 4)
  - [ ] Stage new vectors/metadata before activation.
  - [ ] Switch active source in one step and cleanup old chunks only after success.
- [ ] Persist and propagate source description metadata (AC: 5)
  - [ ] Store description on source records.
  - [ ] Attach description to every chunk metadata object.

## Dev Notes

- Replacement safety is a core correctness requirement; prioritize rollback integrity.
- Avoid partial states by separating staging and activation phases.
- Duplicate detection must be project-scoped, not global.

### Project Structure Notes

- Backend focus: `backend/services/ingestion.py`, source persistence services, Chroma write helpers.
- Frontend focus: source upload/replace controls and job-result handling.

### References

- [Source: docs/epics.md#Epic 4 - Source Management & Replacement Safety]
- [Source: docs/prd.md#5.3 Source Management]
- [Source: docs/architecture.md#4.3 Source]
- [Source: docs/architecture.md#4.4 ChromaDB Chunk (metadata per chunk)]
- [Source: docs/architecture.md#6.1 Ingestion Pipeline]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
