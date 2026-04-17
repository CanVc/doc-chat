# Story 8.1: Backend Security Hardening

Status: ready-for-dev

## Story

As an admin,
I want backend ingestion and API boundaries hardened,
so that DocChat remains safe to run on an internal network.

## Acceptance Criteria

1. File ingestion validates extension, MIME type, and file signature before parser execution.
2. Uploaded file paths are backend-generated and protected against path traversal under `data/uploads/`.
3. CORS is configured with explicit frontend-origin allowlist (no wildcard).
4. Security checks apply consistently across file upload and URL ingestion entrypoints.

## Tasks / Subtasks

- [ ] Implement layered file validation controls (AC: 1)
  - [ ] Check extension and MIME against allowed matrix.
  - [ ] Verify magic bytes/signature for supported binary formats.
- [ ] Implement safe upload path strategy (AC: 2)
  - [ ] Use UUID filenames and canonical path resolution checks.
  - [ ] Reject any path escaping `data/uploads/`.
- [ ] Enforce strict CORS allowlist config (AC: 3, 4)
  - [ ] Configure allowed origins from explicit settings.
  - [ ] Ensure no wildcard fallback in production config.

## Dev Notes

- These controls are mandatory guardrails, not optional hardening.
- Keep security logic centralized and reusable across endpoints.
- Maintain clear error responses for rejected inputs.

### Project Structure Notes

- Backend focus: upload handlers, parser gateway, middleware/config in app bootstrap.
- Relevant modules likely include `backend/main.py`, `backend/api/sources.py`, and ingestion services.

### References

- [Source: docs/epics.md#Epic 8 - Security, Reliability & UX Polish]
- [Source: docs/prd.md#6. Non-Functional Requirements]
- [Source: docs/architecture.md#9. Security & Trust Boundaries]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
