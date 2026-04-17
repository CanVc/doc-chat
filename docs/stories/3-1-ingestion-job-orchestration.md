# Story 3.1: Ingestion Job Orchestration and SSE Progress

Status: ready-for-dev

## Story

As a user,
I want ingestion to run as an asynchronous job with live status events,
so that I can see progress immediately after submitting a source.

## Acceptance Criteria

1. `POST /api/projects/{id}/sources` creates an ingestion job and returns `job_id` with queued status.
2. `GET /api/projects/{id}/sources/jobs/{job_id}/events` streams SSE progress states (`queued`, `parsing`, `chunking`, `embedding`, `storing`, `completed`, `failed`).
3. Ingestion starts within 5 seconds of upload or URL submission under normal conditions.
4. Final completion event includes indexed chunk count and source name.

## Tasks / Subtasks

- [ ] Create ingestion job lifecycle model (AC: 1, 2)
  - [ ] Implement in-memory or persisted job state tracking for active jobs.
  - [ ] Add SSE event emitter abstraction for job status updates.
- [ ] Implement source job creation endpoint (AC: 1, 3)
  - [ ] Validate project existence and enqueue ingestion work quickly.
  - [ ] Return immediate queued response with `job_id`.
- [ ] Implement job events SSE endpoint (AC: 2, 4)
  - [ ] Stream ordered progress and terminal events.
  - [ ] Ensure completion payload includes `chunk_count` and `source_name`.

## Dev Notes

- Keep API responsive by decoupling request handling from heavy ingestion steps.
- SSE contract should be stable for frontend progress components.
- Job orchestration must support both file and URL ingestion paths.

### Project Structure Notes

- Backend APIs: `backend/api/sources.py`.
- Backend services: `backend/services/ingestion.py` and supporting job/event modules.

### References

- [Source: docs/epics.md#Epic 3 - Ingestion Jobs (Files + URL)]
- [Source: docs/prd.md#5.2 Document Ingestion]
- [Source: docs/architecture.md#5. API Routes]
- [Source: docs/architecture.md#6.1 Ingestion Pipeline]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
