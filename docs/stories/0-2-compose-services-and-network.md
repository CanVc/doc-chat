# Story 0.2: Compose Services and Network

Status: ready-for-dev

## Story

As an admin,
I want a single Docker Compose stack with frontend, backend, and ChromaDB,
so that I can start DocChat on a fresh machine with one command.

## Acceptance Criteria

1. `docker compose up` starts `frontend`, `backend`, and `chromadb` services with correct dependencies.
2. Frontend can reach backend over the Docker network, and backend can reach ChromaDB.
3. ChromaDB service exposes a healthcheck used by backend startup ordering.
4. Compose configuration remains v1-simple and environment-driven for ports only.

## Tasks / Subtasks

- [ ] Define `docker-compose.yml` services and networking (AC: 1, 2)
  - [ ] Configure service names, internal connectivity, and `depends_on` semantics.
  - [ ] Expose frontend/backend ports from environment variables.
- [ ] Add ChromaDB health orchestration (AC: 3)
  - [ ] Configure ChromaDB heartbeat healthcheck.
  - [ ] Gate backend startup on healthy ChromaDB.
- [ ] Validate startup behavior on clean clone (AC: 1, 4)
  - [ ] Run local boot test and document expected running containers.

## Dev Notes

- Keep architecture to three services only for v1; do not introduce extra infrastructure.
- Use Compose as the canonical local platform entrypoint.
- Preserve explicit service boundaries: UI -> API -> Vector Store.

### Project Structure Notes

- Primary file: `docker-compose.yml`.
- Service build contexts: `frontend/` and `backend/`.
- ChromaDB uses image-based service and persistent volume.

### References

- [Source: docs/epics.md#Epic 0 - Local Platform & Developer Setup]
- [Source: docs/prd.md#6. Non-Functional Requirements]
- [Source: docs/architecture.md#1. Overview]
- [Source: docs/architecture.md#8. Docker Compose]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
