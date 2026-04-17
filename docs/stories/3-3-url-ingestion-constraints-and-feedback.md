# Story 3.3: URL Ingestion Constraints and Feedback

Status: ready-for-dev

## Story

As a user,
I want to ingest a single web page URL with predictable limits,
so that remote content ingestion is safe and understandable.

## Acceptance Criteria

1. URL ingestion accepts only `http` and `https` schemes.
2. Ingestion enforces timeout 30 seconds, maximum 5 redirects, and maximum page size 50 MB.
3. Scope is single-page only (no crawling, no JS rendering, no authenticated pages).
4. URL-specific failures surface explicit errors (unreachable, timeout, too many redirects, unsupported content type/scheme, size exceeded).
5. Progress and final success details are visible through existing ingestion job stream.

## Tasks / Subtasks

- [ ] Implement URL input validation and fetch constraints (AC: 1, 2, 3)
  - [ ] Reject non-HTTP schemes and malformed URLs early.
  - [ ] Configure HTTP client with timeout, redirect, and content-size guards.
- [ ] Implement URL text extraction pipeline (AC: 3)
  - [ ] Parse fetched HTML/text without JS rendering or link traversal.
  - [ ] Reject authenticated/unsupported fetch contexts as out of scope.
- [ ] Map URL failures to UI-friendly errors and SSE events (AC: 4, 5)
  - [ ] Standardize backend error codes/messages.
  - [ ] Emit failed/completed states with clear payloads.

## Dev Notes

- Use `httpx` for controlled async requests and consistent timeout handling.
- Keep URL ingestion behavior deterministic and auditable.
- Do not add crawler behavior in v1.

### Project Structure Notes

- Core files: `backend/services/parsers/url.py`, `backend/services/ingestion.py`, `backend/api/sources.py`.
- Frontend consumption in source ingestion components for progress and errors.

### References

- [Source: docs/epics.md#Epic 3 - Ingestion Jobs (Files + URL)]
- [Source: docs/prd.md#5.2 Document Ingestion]
- [Source: docs/architecture.md#2. Tech Stack]
- [Source: docs/architecture.md#6.4 Ingestion Limits]
- [Source: docs/architecture.md#9. Security & Trust Boundaries]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
