# PROJECT_CONTEXT.md — Living project state

> **Agents: read this first. Replace this whole file after finishing any task or at session end.**

Last updated: 2026-11-12 (by: planning session, no code yet)

## Current Status
- Phase: Planning complete, Phase 1 / Week 1 not started.
- Code written: none.

## Project Summary
OpsPilot AI: agentic incident investigation + guarded Kubernetes remediation. Details in `docs/usecase.md`, design in `docs/ARCHITECTURE.md`, plan in `docs/ROADMAP.md`.

## Decisions (locked)
- Time budget: 4 hrs/day, 10-week plan (5 MVP + 5 hardening).
- Stack: Python, FastAPI, PostgreSQL + pgvector, SQLAlchemy/Alembic, LangGraph, React + TS, Docker Compose, kind, Prometheus, Grafana, GitHub Actions.
- Deferred: Terraform, OpenTelemetry, multi-tenancy, Redis.
- Safety: policy engine in code is the authority; LLM never grants permission.
- Multiple AI agents are used across sessions; docs are the shared memory.

## Done
- Project selection, scope, architecture, roadmap, agent rules, use case written.

## In Progress
- Nothing yet.

## Next
- Week 1 Day 1: repo skeleton, git init, Docker Compose (Postgres + FastAPI), `/health`, first commit.

## Known Issues
- None.

## How to Run
- Not set up yet. (Update after Day 1.)

## Session Handoff
- Start with Week 1 Day 1 in `docs/ROADMAP.md`. Add the first learning-log entry for each file/service created.
