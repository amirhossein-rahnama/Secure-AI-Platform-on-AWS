# Secure AI Platform on AWS

The platform an AI team would actually run on, built the way a careful platform engineer would build it: infrastructure as code, GitOps delivery, DevSecOps gates, least-privilege security, SLOs and cost controls, on AWS.

> **Status:** early work in progress (kickoff, October 2026). This README describes the target platform; the roadmap below shows what exists so far.

## Scope

| Area | What the platform will provide |
|---|---|
| Infrastructure as code | Terraform modules for networking (VPC), compute (ECS on Fargate first, then Amazon EKS), IAM and KMS; remote state with locking; separate `dev` and `prod` environments; `terraform test` for the modules |
| Delivery | GitHub Actions authenticating to AWS through OIDC (no stored keys); reusable workflows and a manual approval gate for `prod`; GitOps with Argo CD and Helm; canary releases with Argo Rollouts |
| DevSecOps gates | SAST with Semgrep; Trivy scans of images, dependencies and Terraform; secret scanning with gitleaks; an SBOM for every image; images signed with cosign and verified at admission by Kyverno |
| Security | Least-privilege workload identities, AWS Secrets Manager, network policies, Pod Security Standards, Amazon GuardDuty |
| Reliability | OpenTelemetry into Prometheus and Grafana, two SLOs with error-budget alerts, a runbook, a chaos experiment with AWS Fault Injection Service, and a blameless postmortem |
| Cost | Cost-allocation tags, an AWS Budgets alarm, a per-service cost dashboard, and a teardown script that keeps the bill near zero between demos |

## Roadmap

- [ ] **Kickoff:** repository, a containerised Python service with tests and a Makefile, first C4 context diagram
- [ ] **Architecture:** C4 container diagram and the first architecture decision records (ADRs)
- [ ] **v1:** Terraform foundation (remote state, VPC, IAM, ECR, ECS on Fargate), CI/CD with GitHub Actions and OIDC, budget alarm and cost tags
- [ ] **v2:** move workloads to Amazon EKS with Helm, Argo CD and canary rollouts
- [ ] **Security gates:** supply-chain checks in CI, signed images verified by Kyverno, network policies, Pod Security Standards
- [ ] **v3:** SLOs and alerting, observability dashboards, a chaos experiment and postmortem, cost dashboard

## Principles

- **No long-lived credentials.** People sign in through IAM Identity Center, CI assumes roles through OIDC, and workloads run with their own narrowly scoped, short-lived roles.
- **Everything as code.** Infrastructure, policies, dashboards and alerts live in this repository and are reviewed like any other change.
- **Decisions are written down.** Every significant choice gets an ADR covering the context, the options and the trade-offs.
- **Pay only while it runs.** Environments are disposable, and one command tears everything down.
- **Runnable by a stranger.** Prerequisites, setup, teardown and expected costs are documented.

## Documentation

Architecture decision records will live in `docs/adr/` and diagrams in `docs/architecture/`. The threat model, runbook and postmortem will be linked here as they are written.

## Getting started

Not runnable yet. Prerequisites and one-command setup and teardown instructions arrive with v1.
