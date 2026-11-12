# OpsPilot AI — Roadmap (10 weeks, ~4 hrs/day, ~280 hrs)

Tick items as they finish: `[x]`. Phase 1 = MVP (resume-ready at end of Week 5). Phase 2 = hardening (Weeks 6–10).

Daily 4-hour split: ~1.5h build, ~1h debug/understand, ~0.5h tests, ~0.5h learning log + docs, ~0.5h commit/review.
Rule: every day ends with a working commit, a documented bug fix, or a tested feature.

## Phase 1 — MVP

### Week 1 — Foundation
- [ ] D1: repo skeleton, docs, git init, Docker Compose (Postgres + FastAPI), `/health`, first commit
- [ ] D2: SQLAlchemy models + Alembic migrations (users, services, incidents, events)
- [ ] D3: JWT auth + roles (viewer/engineer/admin)
- [ ] D4: incident CRUD APIs + tests
- [ ] D5: audit_logs table + write on every state change
- [ ] D6: sample payment service (/health, /metrics, failure toggles) in Docker
- [ ] D7: orders service + review week, update context
**Deliverable:** containerized backend, DB, sample services.

### Week 2 — Monitoring + alert ingestion
- [ ] Prometheus + Grafana in Compose, scrape sample services
- [ ] Alert rules + Alertmanager webhook -> `POST /alerts` -> incident
- [ ] Failure injection script (crash, high latency, bad config)
- [ ] Log access (container logs via tool, later Loki)
- [ ] Grafana dashboard for services
**Deliverable:** failure detected automatically, incident created.

### Week 3 — Agentic AI core
- [ ] LLM client (configurable provider), structured outputs (Pydantic)
- [ ] LangGraph basics: triage agent
- [ ] Log analysis agent + read-only tools
- [ ] RCA agent with evidence requirement
- [ ] Runbooks written + pgvector RAG ingest/retrieve
- [ ] Persist agent runs/events
**Deliverable:** investigation pipeline producing evidence-backed RCA.

### Week 4 — Policy engine + remediation
- [ ] Policy engine (roles, allowlist, risk levels, arg validation, limits)
- [ ] Input guardrails: injection detection, PII masking
- [ ] Remediation agent proposes actions; stored in `proposed_actions`
- [ ] Human approval via LangGraph interrupt + approve/reject APIs
- [ ] kind cluster, deploy samples, Kubernetes tool layer (read + pod restart)
**Deliverable:** approved remediation executes on kind.

### Week 5 — Dashboard + MVP launch
- [ ] React dashboard: incident list, detail, timeline, approvals
- [ ] Recovery verification + post-incident report
- [ ] GitHub Actions CI (lint, test, build)
- [ ] End-to-end demo working
- [ ] README, finalize architecture docs, demo video
**Deliverable:** MVP. Add to resume as "MVP", no unmeasured claims.

## Phase 2 — Hardening

### Week 6 — Testing
- [ ] Pytest coverage on API, policy, agents (mock LLM)
- [ ] Failure handling, retries, malformed LLM output
- [ ] Locust load test, baseline report

### Week 7 — Security
- [ ] Stronger injection defences, untrusted-log tests
- [ ] Unauthorized tool-call tests, execution budgets, rate limits
- [ ] Secrets handling, k8s service account least privilege
- [ ] Security test suite with detection + false-positive measurement

### Week 8 — Observability + deployment
- [ ] Loki logs, improved alerts, ops dashboards
- [ ] k8s probes/limits, rollback, CI quality gates
- [ ] Optional: OpenTelemetry, Terraform (only if time allows)

### Week 9 — Evaluation
- [ ] 30–50 simulated incidents with labels
- [ ] Measure classification/RCA accuracy, remediation success, MTTR vs baseline, latency, cost

### Week 10 — Release
- [ ] Fix remaining critical issues
- [ ] Final docs, architecture diagram, results write-up
- [ ] Demo video, resume bullets with real numbers, interview prep (explain without AI)

## Interview-readiness checklist
- [ ] Explain full architecture without AI help
- [ ] Debug a broken API/container/k8s deployment
- [ ] Explain LangGraph design choices
- [ ] Demo prompt injection being blocked
- [ ] Explain how you know an incident was actually resolved
