# STRATEGY: AI Native Task Manager — Solo Execution Plan (12 Weeks)

**Author:** Allah Bakhsh(Multan, PK)  
**Start Date:** Today (Week 0 ends 13 April 2026)  
**Target Completion:** 30 June 2026 (Week 12)  
**Goal:** Build a production-grade Kubernetes project that proves you master:

- Microservices architecture
- AI agent workloads
- Zero-trust security (RBAC + NetworkPolicy + SecurityContext)
- Observability & scaling
- "Comment-by-comment" documentation

---

## Use Case (why this project exists)

A smart task manager where:

- Users create/view tasks via a clean React UI.
- The Todo Agent (powered by Claude Sonnet 4) autonomously reads tasks, decides priorities, creates subtasks, updates status, and even closes tasks — all without human input.
- Backend + Postgres handles persistence.
- Notification Service sends email/push alerts.
- Everything runs securely in Kubernetes with proper scaling, probes, quotas, and defense-in-depth.

This is real software, not a toy. Recruiters will see you can design, build, secure, and document a full AI-native system.

---

## Success = This GitHub Repo

When finished, your repo must contain:

- Every YAML from the PLAN.md as real files (with comments linking back to PLAN.md section)
- Working services with inline comments
- CI/CD that builds & scans images
- A 5-minute Loom video walkthrough
- Zero warnings on `kubectl apply --dry-run=server`

---

## 12-Week Timeline (Realistic for Solo, 10–15 hrs/week)

| Week | Phase | Focus | Est. Hours | Deliverable |
|------|-------|-------|------------|-------------|
| 0 (now) | Setup | Repo + tools | 4 | Repo created |
| 1 | Local Env | Docker Compose | 12 | All services run locally |
| 2–3 | Services | Code each component | 25 | 4 working services |
| 4–5 | Docker + K8s | Manifests + local cluster | 20 | `kubectl apply -f k8s/` works |
| 6 | Security | RBAC + NetworkPolicy + Secrets | 15 | Full zero-trust |
| 7 | Scaling & Observability | HPA + probes + logs | 12 | Auto-scaling demo |
| 8 | CI/CD | GitHub Actions | 10 | Automatic builds |
| 9 | Polish & Docs | README + comments | 10 | "Comment-by-comment" complete |
| 10 | Cloud Deploy | Cheap K8s cluster | 12 | Live demo URL |
| 11 | Video + Proof | Loom + portfolio page | 8 | Recruiter-ready |
| 12 | Buffer & Ship | Final fixes | 8 | Public launch |

**Total: ~136 hours → very doable at 12 hrs/week.**

---

## Detailed Step-by-Step Strategy (Follow Exactly)

### Week 0 – Setup (Today – 13 April)

1. Create GitHub repo: `ai-native-task-manager` (public, add license MIT)
2. Clone it locally.
3. Copy the original PLAN.md into the root.
4. Create these folders (exact structure):

```
├── PLAN.md
├── STRATEGY.md          ← paste this whole document
├── README.md            ← (I'll give you the hero version next)
├── .github/workflows/
├── k8s/base/            ← all raw manifests
├── k8s/overlays/dev/
├── services/
│   ├── ui-interface/
│   ├── backend-api/
│   ├── todo-agent/
│   └── notification-service/
├── docker-compose.yml
└── Taskfile.yml         ← one-command dev
```

5. Install tools (all free):
   - Docker Desktop or Rancher Desktop (you already know Rancher)
   - kubectl + kind or Minikube
   - Task – makes dev work
   - GitHub CLI

**Commit:** `chore: initial repo structure per STRATEGY.md`

> **End of week proof:** Repo exists with folders.

---

### Week 1 – Local Environment (14–20 April)

1. Write `docker-compose.yml` (4 services + Postgres).
2. Create minimal Dockerfile in each service folder.
3. Run `task up` → all services healthy.
4. Test basic flow: UI → Backend → Agent.
5. Commit every day with message: `feat(service): add X (see PLAN.md section Y)`

> **Milestone:** `docker compose up` shows all 4 services + DB running.

---

### Weeks 2–3 – Build the Four Services (21 Apr – 4 May)

**Rule:** Every file you create must have comments referencing PLAN.md.

- **UI** (React/Next.js) → use PLAN.md 2.1 specs
- **Backend** → Node.js or Python FastAPI + Prisma/TypeORM
- **Todo Agent** → Python + LangChain/Anthropic SDK (most comments here)
- **Notification** → simple queue + SendGrid

**Daily rule:** 1–2 hours coding + 30 min writing comments.  
**Commit rule:** Reference exact section (e.g. `"securityContext as per PLAN.md 2.3"`)

---

### Weeks 4–5 – Kubernetes Manifests (5–18 May)

1. Create all YAMLs in `k8s/base/` exactly as described in PLAN.md sections 1–7.
2. Use kind or Minikube cluster locally.
3. `kubectl apply -f k8s/base/` must succeed.
4. Test probes, services, DNS inside cluster.

---

### Week 6 – Security Layer (19–25 May)

Apply exactly:

- ServiceAccounts + Roles + RoleBindings
- NetworkPolicy (default-deny)
- SecurityContext on Agent
- External Secrets placeholder (commented)

---

### Week 7 – Scaling & Observability (26 May – 1 Jun)

1. Add HPA for backend & agent (memory-based for agent)
2. Add Prometheus + Grafana simple scrape
3. Test scaling manually

---

### Week 8 – CI/CD (2–8 Jun)

**GitHub Actions:**

- Build & scan Docker images on push
- Trivy vulnerability scan
- Optional: push to ghcr.io

---

### Weeks 9–10 – Polish + Real Cloud (9–22 Jun)

1. Write full "comment-by-comment" documentation
2. Deploy to Hetzner Cloud Kubernetes (₹800–1200/month, cheapest real cluster)
3. Get a public URL via Ingress

---

### Weeks 11–12 – Proof & Launch (23 Jun – 6 Jul)

1. Record 5–8 min Loom video walking through repo + live demo.
2. Update README with:
   - Use case
   - Architecture diagram
   - Link to every PLAN.md section → actual code
   - "What I learned" section
3. Pin the repo. Add topics: `kubernetes`, `ai-agent`, `microservices`, `zero-trust`, `claude-ai`

---

## Anti-Distraction Rules (Solo Developer Edition)

1. **One phase at a time.** No jumping.
2. **Daily commit before 11 PM PKT** (even if tiny).
3. **Weekly review every Sunday 9 PM:** Reply here with `"Week X complete ✅"` and I will review + give next files.
4. **If stuck > 2 hours → message me immediately** (I will unblock in < 30 min).
5. **No new features until Week 12.** Scope is frozen to PLAN.md.
