# Plan 1: AI Native Task Manager — Kubernetes Deployment Plan

**Author:** Allah (Multan, Pakistan)  
**Date:** April 2026  
**Status:** Approved → In Progress → Implementation (this repo is the living proof)

## Executive Summary
An AI-native task management platform where a smart **Todo Agent** (powered by Claude Sonnet 4) autonomously manages tasks — creating, prioritizing, breaking down, and closing them with minimal human input. The system consists of four microservices deployed securely on Kubernetes.

**Core Use Case:**
- Users interact with a clean React UI to create and view tasks.
- The Todo Agent continuously polls for new/open tasks, uses LLM reasoning to decide actions, updates task status in the backend, and triggers notifications.
- Backend handles persistence (PostgreSQL) and business logic.
- Notification Service sends emails/push updates via SendGrid/SMTP.
- Everything runs with production-grade scaling, security, and observability.

## Why This Project Proves My Skillset
- End-to-end Kubernetes architecture for microservices + AI agent workload
- Correct distinction between stateless Deployments and stateful components
- AI-specific scaling (memory-based HPA for the agent)
- Zero-trust security model (RBAC least privilege, NetworkPolicy default-deny, hardened SecurityContext)
- Proper ConfigMap vs Secret separation + rotation strategy
- Self-documenting code with comments linking directly to this plan
- Full CI/CD and cloud deployment path as a solo engineer

## Project Phases (Roadmap)
See `STRATEGY.md` for the detailed 12-week execution plan.

## 1. Namespace

All workloads run inside a dedicated namespace:

**Namespace:** `task-manager`

**ResourceQuota:** `task-manager-quota`
- pods: 20
- requests.cpu: 4
- requests.memory: 8Gi
- limits.cpu: 12
- limits.memory: 20Gi
- secrets: 20

> **🧠 Design Thinking:** Single namespace keeps RBAC and lifecycle simple for tightly coupled components. The quota prevents the AI agent's HPA from starving other services.

## 2. Pods and Deployments

### 2.1 UI Interface — Deployment
- Replicas: 2 (stateless)
- Image: `ui-interface:latest`
- Port: 3000
- Resources: CPU 100m/300m, Memory 128Mi/256Mi
- Probes: liveness `/health` (10s), readiness `/ready` (5s, initialDelay 5s)
- EnvFrom: `ui-config`, `ui-secrets`
- ServiceAccount: `ui-sa` (no K8s API access)

### 2.2 Backend APIs — Deployment
- Replicas: 3 + HPA (3→10 at 70% CPU)
- Image: `backend-api:latest`
- Port: 8080
- Resources: CPU 250m/1000m, Memory 256Mi/512Mi
- Probes: liveness `/health` (10s), readiness `/ready` (5s, initialDelay 10s)
- EnvFrom: `backend-config`, `backend-secrets`
- ServiceAccount: `backend-api-sa`

### 2.3 Todo Agent — Deployment
- Replicas: 2 + HPA (2→6 at 75% memory)
- Image: `todo-agent:latest`
- Port: 9000 (internal)
- Resources: CPU 500m/2000m, Memory 512Mi/2Gi
- SecurityContext: runAsNonRoot, readOnlyRootFilesystem, drop ALL capabilities
- EnvFrom: `agent-config`, `agent-secrets`
- ServiceAccount: `todo-agent-sa`
- **Why memory HPA:** LLM agents are memory-bound due to token buffers and history.

### 2.4 Notification Service — Deployment
- Replicas: 2
- Image: `notification-service:latest`
- Port: 8085
- Resources: CPU 100m/500m, Memory 128Mi/256Mi
- Probes: liveness `/health`, readiness `/ready`
- EnvFrom: `notification-config`, `notification-secrets`
- ServiceAccount: `notification-sa`

### 2.5 PostgreSQL (External Dependency)
Acknowledged as StatefulSet or managed service (e.g. Supabase / Neon / RDS). Referenced via `postgres-service.task-manager.svc.cluster.local`.

## 3. Services

| Service Name              | Type          | Port Mapping      | Purpose                     |
|---------------------------|---------------|-------------------|-----------------------------|
| `ui-service`              | LoadBalancer  | 80 → 3000         | External user access        |
| `backend-api-service`     | ClusterIP     | 8080              | Internal API calls          |
| `todo-agent-service`      | ClusterIP     | 9000              | Internal agent communication|
| `notification-service`    | ClusterIP     | 8085              | Internal notifications      |

**Production note:** Replace UI LoadBalancer with Ingress + TLS.

## 4. ConfigMaps

- `ui-config`
- `backend-config`
- `agent-config`
- `notification-config`

(Full content remains exactly as in your original plan — service URLs use short DNS names.)

## 5. Secrets Management

Secrets: `backend-secrets`, `agent-secrets`, `notification-secrets`, `ui-secrets`

**Recommended production approach:** External Secrets Operator (ESO) + Stakater Reloader for rotation.

## 6. RBAC

- Dedicated ServiceAccounts for each component (`ui-sa`, `backend-api-sa`, `todo-agent-sa`, `notification-sa`)
- Minimal Role/RoleBinding for `todo-agent-sa` (only ConfigMap get/list/watch)
- Minimal Role/RoleBinding for `backend-api-sa` (ConfigMap + Endpoints)
- No unnecessary permissions. No `pods/exec` for anyone.

## 7. NetworkPolicy

Default deny-all + explicit allowlist:
- Internet → UI only
- UI → Backend & Agent
- Agent → Backend
- Backend → Notification & Postgres
- Agent → Anthropic API (egress)
- Notification → SMTP/SendGrid (egress)
- Block all pods to Kubernetes API server

## 8. Summary Table

(See original plan — 1 Namespace, 4 Deployments, 4 Services, 4 ConfigMaps, 4 Secrets, etc.)

---

**Implementation Note:**  
Every Kubernetes manifest and source file in this repository contains inline comments that directly reference the relevant section of this PLAN.md. This makes the repo self-documenting proof of architectural understanding.