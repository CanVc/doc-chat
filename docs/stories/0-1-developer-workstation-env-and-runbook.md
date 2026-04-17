# Story 0.1: Developer Workstation, Env Template, and Runbook

Status: ready-for-dev

## Story

As a developer,
I want explicit workstation prerequisites and setup instructions,
so that I can run DocChat in Docker and local dev mode without guesswork.

## Acceptance Criteria

1. README documents required workstation tooling: `Docker` + Compose v2, `Python 3.11`, `venv`, and `Node.js`/`npm`.
2. README includes backend local setup commands using `python -m venv .venv` and dependency installation.
3. README includes frontend local setup commands for package install and dev run.
4. `.env.example` contains only infrastructure settings (ports and volume paths if applicable).
5. Documentation explicitly states that provider credentials are configured in Settings, not `.env`.
6. New contributors can follow docs to boot the stack without hidden steps.

## Tasks / Subtasks

- [ ] Document workstation prerequisites and verification commands (AC: 1, 6)
  - [ ] Add minimum versions and OS-agnostic checks (`docker --version`, `docker compose version`, `python --version`, `node --version`).
  - [ ] Include troubleshooting notes for missing prerequisites.
- [ ] Document local backend and frontend setup flows (AC: 2, 3, 6)
  - [ ] Add backend setup with `python -m venv .venv`, activation, and install command.
  - [ ] Add frontend setup with package manager install and dev command.
- [ ] Create/normalize `.env.example` to infra-only keys (AC: 4)
  - [ ] Remove any API-key-like variables from template.
  - [ ] Keep clear comments for port/path configuration.
- [ ] Write startup runbook in README (AC: 6)
  - [ ] Add clone, env copy, and compose startup commands.
  - [ ] Add verification checks for running services.
- [ ] Clarify provider-secret policy (AC: 5)
  - [ ] Document that OpenAI/Ollama configuration lives in Settings UI.

## Dev Notes

- Keep onboarding under two minutes for first successful boot once prerequisites are installed.
- Docs must stay aligned with v1 out-of-scope boundaries.
- Language must remain English across project documentation.

### Project Structure Notes

- Update `README.md` and `.env.example` only.
- Do not add secret-management complexity in v1.

### References

- [Source: docs/epics.md#Epic 0 - Local Platform & Developer Setup]
- [Source: docs/prd.md#2. Goals & Success Criteria]
- [Source: docs/prd.md#6. Non-Functional Requirements]
- [Source: docs/architecture.md#8. Docker Compose]

## Dev Agent Record

### Agent Model Used

TBD

### Debug Log References

### Completion Notes List

### File List
