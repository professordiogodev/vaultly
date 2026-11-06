# Project 3 — Option B: Vaultly

## Concept

Build and operate a **security-first secrets management platform** on cloud infrastructure. Instead of broad DevOps coverage, this option goes deep on the thing that most teams get wrong: secrets, access control, and compliance.

## The application

**Vaultly**: https://github.com/professordiogodev/vaultly — a minimal secrets API
wrapper (FastAPI) that stores and retrieves secrets via a REST API, backed by **HashiCorp Vault**.
It already supports Vault's Kubernetes auth method (no static tokens), and ships a live
database-credential rotation demo: change a secret in Vault and a running instance picks up the
new value on its next poll, with no restart. Read its README for the local dev quickstart
(`docker compose up --build`) before you touch infrastructure.

You are not building this app — clone it and treat it as a black box you need to run securely.
Your job is everything *around* it: deploy Vault in HA mode on Kubernetes, wire up auto-unseal,
get Vaultly authenticating against a real cluster via the Kubernetes auth method, provision the
surrounding cloud infrastructure with Terraform, and build the CI/CD security pipeline that ships
it. The requirements below are what "securely" means for this project — Vaultly's own README spells
out which of them (like non-root containers and no-secrets-in-logs) it already handles at the
application level, and which ones are explicitly left to this deployment.

Alternatively: take `taskly` (https://github.com/professordiogodev/taskly/)
and retrofit it to read all configuration from Vault, demonstrating the secrets lifecycle in a real
app.

## Requirements

### Secrets management
- [ ] HashiCorp Vault (or equivalent) deployed on Kubernetes with HA mode (3 replicas, Raft storage)
- [ ] Vault auto-unseal using AWS KMS or Azure Key Vault
- [ ] Application authenticates to Vault using Kubernetes auth method (not static tokens)
- [ ] Secrets rotated: database password rotation demonstrated (change in Vault → app picks up new credentials without restart)
- [ ] All secrets encrypted at rest and in transit

### Infrastructure security
- [ ] All resources provisioned with Terraform
- [ ] Security group / NSG rules: principle of least privilege (document why each rule exists)
- [ ] VPC with private subnets for database and Vault; only load balancer in public subnet
- [ ] AWS GuardDuty or Microsoft Defender for Cloud enabled; show the console
- [ ] CloudTrail or Azure Activity Log exported to S3 / Storage Account; write one KQL or Athena query against them

### CI/CD security
- [ ] GitHub Actions pipeline with Trivy scanning (block on CRITICAL)
- [ ] SAST scan (Semgrep or Bandit for Python)
- [ ] Dependency scanning (pip-audit or Dependabot)
- [ ] No secrets in pipeline environment variables — all pulled from Vault or Secrets Manager at runtime
- [ ] OIDC-based authentication from GitHub Actions to cloud (no long-lived access keys)

### Access control
- [ ] IAM / Entra ID: demonstrate least-privilege role assignments with evidence (show what the app role CAN'T do)
- [ ] Kubernetes RBAC: service accounts with minimal permissions; no pods running as root
- [ ] MFA enabled on all human accounts used for the demo

### Documentation
- [ ] `README.md`: threat model (what attacks does this architecture resist?)
- [ ] Architecture diagram with trust boundaries marked
- [ ] Secrets rotation runbook

## What makes this option the right choice

Pick this if you want to specialise in security engineering or DevSecOps. This option trades breadth for depth on the thing that causes the most production incidents: credential compromise and misconfigured access.
