# OpsPilot AI — Architecture

Diagram: see `docs/architecture-diagram.md`.

## 1. Components
| Component | Tech | Responsibility |
|---|---|---|
| Sample services | FastAPI (payment, orders) | Targets for failure injection; expose /health and /metrics |
| Monitoring | Prometheus + Grafana (+ Alertmanager webhook) | Metrics, alerts, dashboards |
| Backend API | FastAPI | Auth, incidents, approvals, audit, orchestration entry |
| Database | PostgreSQL 16 + pgvector | Incidents, users, actions, audit, runbook embeddings |
| Agent layer | LangGraph | Triage, log analysis, RCA, remediation, verification |
| RAG | pgvector + embeddings | Runbook retrieval |
| Policy engine | Python module | Permission, risk class, allowlist, arg validation, limits |
| Tool layer | Python + Kubernetes client | Read-only diagnostics; allowlisted write actions |
| Input guardrails | Python | Injection detection, PII masking, validation |
| Dashboard | React + TypeScript | Incidents, evidence, approvals, history |
| CI/CD | GitHub Actions | Lint, test, build images |

## 2. Request flow
1. Alert (Prometheus/Alertmanager webhook or health poller) hits `POST /alerts`.
2. Backend creates an incident (status `open`).
3. Input guardrails sanitize/flag log and alert content.
4. LangGraph run starts: triage → investigate (logs + metrics tools, RAG) → RCA → remediation proposal.
5. Proposal is stored in `proposed_actions`; policy engine assigns risk and decision (`allowed`, `needs_approval`, `denied`).
6. If `needs_approval`, the graph pauses (LangGraph interrupt). Authorized user approves/rejects in the dashboard.
7. On approval the tool layer executes the action (policy engine re-checks at execution time).
8. Verification node checks health/metrics; incident becomes `resolved` or `escalated`.
9. Report generated. Every step writes to `audit_logs`.

## 3. Safety layer (critical boundary)
```
LLM output -> Pydantic schema validation -> Policy engine -> Tool layer -> Kubernetes
```
- LLM never grants itself permission. Policy engine is deterministic code.
- Policy engine checks: user/agent role, tool allowlist, argument validation (namespace/pod allowlist), risk level, approval status, rate/step/token limits.
- Execution re-validates policy right before acting (no stale approvals).

### Risk levels
| Level | Examples | Rule |
|---|---|---|
| read | get logs, describe pod, query metrics | auto-allowed |
| low | restart stateless pod | engineer approval |
| medium | scale deployment | engineer approval |
| high | touch database pod, delete anything, change config | admin approval |
| forbidden | delete namespace, access secrets, exec into pod | always denied |

## 4. Database schema (initial)
- `users` (id, email, password_hash, role, created_at)
- `services` (id, name, namespace, health_url, created_at)
- `incidents` (id, service_id, title, severity, category, status, root_cause, confidence, created_at, resolved_at)
- `incident_events` (id, incident_id, type, payload JSON, created_at)  — timeline
- `agent_runs` (id, incident_id, graph_state JSON, status, tokens_used, started_at, ended_at)
- `proposed_actions` (id, incident_id, tool, args JSON, risk_level, policy_decision, status, proposed_at)
- `approvals` (id, action_id, user_id, decision, comment, decided_at)
- `audit_logs` (id, actor_type, actor_id, event, details JSON, created_at)
- `runbook_chunks` (id, source, content, embedding vector, created_at)
- `security_events` (id, incident_id, kind, snippet, created_at)

## 5. API (initial)
| Method | Path | Purpose |
|---|---|---|
| GET | /health | Liveness |
| POST | /auth/register, /auth/login | Auth (JWT) |
| POST | /alerts | Alert webhook -> create incident |
| GET | /incidents, /incidents/{id} | List/detail with timeline |
| POST | /incidents/{id}/investigate | Trigger agent run |
| GET | /incidents/{id}/actions | Proposed actions |
| POST | /actions/{id}/approve, /actions/{id}/reject | Human approval |
| GET | /audit | Audit log (admin) |
| POST | /runbooks/ingest | Load runbooks into pgvector |

## 6. Repo structure
```
opspilot-ai/
├── AGENTS.md, CLAUDE.md
├── docs/                 # context, usecase, architecture, roadmap, learning log
├── backend/              # FastAPI app, models, auth, routers, alembic
├── agents/               # LangGraph graphs, nodes, tools, RAG
├── policy/               # policy engine, guardrails
├── services/             # sample microservices
├── frontend/             # React dashboard
├── infra/                # docker-compose, k8s manifests, prometheus, grafana
├── runbooks/             # markdown runbooks
├── evals/                # incident simulations, security test cases
├── tests/
├── .github/workflows/
├── .env.example
└── README.md
```
