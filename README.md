# AI Native Task Manager

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Claude AI](https://img.shields.io/badge/Claude-FF6B6B?logo=anthropic&logoColor=white)

**A production-grade AI-native task manager where an autonomous Todo Agent intelligently manages tasks using Claude Sonnet 4.**

Live Demo: (coming in Week 10)  
Video Walkthrough: ( Loom link — will be added in Week 11 )

## 🎯 Project Goal & Use Case

This is not a simple todo app.  
Users create tasks through a clean UI. The **Todo Agent** runs continuously, reads open tasks from the backend, reasons using LLM, decides what to do (prioritize, break into subtasks, update status, mark complete), and triggers notifications — all autonomously.

**Key Features:**
- React frontend for task management
- FastAPI / Node backend with PostgreSQL
- Autonomous AI Todo Agent (Claude-powered)
- Email notifications via SendGrid
- Full Kubernetes deployment with security hardening

## Why This Proves My Kubernetes & AI Engineering Skillset

- Correct decomposition of stateless vs AI workloads
- Memory-based HPA for the LLM agent (not naive CPU scaling)
- Zero-trust security: least-privilege RBAC, default-deny NetworkPolicy, read-only root filesystem on agent
- Proper ConfigMap / Secret separation + rotation strategy (ESO + Reloader)
- Production-ready probes, resource requests/limits, and ResourceQuota
- "Comment-by-comment" documentation linking code directly to architecture decisions

## Architecture Overview

See full details in [PLAN.md](PLAN.md).

High-level flow: