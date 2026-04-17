# Story 3.2: File Ingestion Validation and Parsing

Status: ready-for-dev

## Story

As a user,
I want to upload supported document files with strict validation,
so that ingestion is reliable and errors are clear when a file is invalid.

## Acceptance Criteria

1. Endpoint accepts one or multiple files in a single request.
2. Supported formats are PDF, DOCX, MD, TXT, and CSV only.
3. Each file enforces maximum size of 50 MB.
4. Parsing pipeline extracts text and chunks content for embedding.
5. Validation/parsing failures return clear user-facing error categories.

## Tasks / Subtasks

- [ ] Implement multi-file request handling (AC: 1)
  - [ ] Accept and iterate files as independent ingestion units.
  - [ ] Aggregate per-file success/failure outcomes.
- [ ] Implement format and size validation (AC: 2, 3)
  - [ ] Validate extension and parser availability for each file.
  - [ ] Reject files larger than 50 MB with explicit error code/message.
- [ ] Implement parsing and chunking workflow (AC: 4, 5)
  - [ ] Route by parser type (`pdf`, `docx`, `text`, `csv`, `md`).
  - [ ] Surface errors for unsupported format, empty document, and parse failures.

## Dev Notes

- Keep parser implementations modular under `services/parsers`.
- Error messaging should match PRD language and remain actionable.
- Prepare metadata output needed by source management stories.

### Project Structure Notes

- Backend files: `backend/services/parsers/*`, `backend/services/ingestion.py`, `backend/api/sources.py`.
- Parser interfaces should return normalized text/chunk structures.

### References

- [Source: docs/epics.md#Epic 3 - Ingestion Jobs (Files + URL)]
- [Source: docs/prd.md#5.2 Document Ingestion]
- [Source: docs/architecture.md#2. Tech Stack]
- [Source: docs/architecture.md#3. Project Structure]
- [Source: docs/architecture.md#6.1 Ingestion Pipeline]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
