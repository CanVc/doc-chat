# Story 7.1: Usage Accounting and Cost Estimation API

Status: ready-for-dev

## Story

As an admin,
I want token usage and cost aggregation per project and globally,
so that I can monitor LLM consumption and spend.

## Acceptance Criteria

1. Backend records usage telemetry from chat requests with project association.
2. `GET /api/settings/usage` returns usage totals per project and global totals.
3. OpenAI usage includes USD estimated cost based on configured pricing rules.
4. Ollama usage returns token counts without cost estimate.
5. Requests missing provider usage metadata are marked unavailable and excluded from estimated cost totals.

## Tasks / Subtasks

- [ ] Implement usage event capture model (AC: 1)
  - [ ] Persist prompt/completion/total tokens and provider/model metadata.
  - [ ] Link records to project IDs and request timestamps.
- [ ] Implement usage aggregation endpoint (AC: 2)
  - [ ] Compute per-project and global totals.
  - [ ] Return stable schema for UI consumption.
- [ ] Implement provider-specific cost logic and missing-data handling (AC: 3, 4, 5)
  - [ ] Apply OpenAI pricing table mapping to token usage.
  - [ ] Set unavailable markers and exclude null-usage rows from cost sums.

## Dev Notes

- Cost math must remain transparent and reproducible.
- Missing usage should never silently skew totals.
- Keep storage simple and local for v1 scope.

### Project Structure Notes

- Backend: chat service emission hooks, usage service/store, `backend/api/settings.py`.
- Pricing configuration can live in code/constants for v1.

### References

- [Source: docs/epics.md#Epic 7 - Usage & System Status]
- [Source: docs/prd.md#5.5 Administration]
- [Source: docs/architecture.md#Chat SSE Contract (POST /api/chat)]
- [Source: docs/architecture.md#5. API Routes]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
