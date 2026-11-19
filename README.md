# Security Study — Multi-Tenant Team Notes & File Vault API

A hands-on security engineering project built from scratch following a structured autonomy roadmap (**TypeScript + NestJS + AppSec + Defensive GCP**).

## Anchor Project

Build a backend API where organizations and users manage private notes and file attachments with strict tenant isolation, role-based access, and auditability. Every security control is implemented by hand first, then hardened in GCP.

## Milestones

### Milestone 0 — TypeScript & NestJS Foundations
- [ ] Initialize NestJS with strict TypeScript (`"strict": true`, `"noUncheckedIndexedAccess": true`, zero `any`)
- [ ] Modular boundaries (`UsersModule`, `NotesModule`, `DatabaseModule`)
- [ ] PostgreSQL via Prisma (`PrismaService`) with parameterized queries only
- [ ] Global `ValidationPipe` (`whitelist: true`, `forbidNonWhitelisted: true`)
- [ ] Integration tests (`jest` + `supertest`) covering CRUD and rejecting malformed/extra JSON fields

### Milestone 1 — Authentication, Sessions & Access Control (AppSec Core)
- [ ] Password hashing with **Argon2id**
- [ ] Short-lived access tokens + rotating refresh tokens (hashed in PostgreSQL, revocable on logout/reuse)
- [ ] `AuthGuard` + `RolesGuard` + resource-ownership / tenant-membership checks
- [ ] E2E tests proving IDOR/BOLA is blocked (`403`/`404`)
- [ ] Self-audit against **OWASP ASVS V2 & V4** Level 1

### Milestone 2 — Application Hardening & Attack Verification
- [ ] Security headers (`helmet`) and strict CORS origin allowlist
- [ ] Rate limiting (`@nestjs/throttler`) on auth and sensitive endpoints
- [ ] Safe file upload endpoint (size limits, magic-byte/MIME validation, random UUID object keys)
- [ ] Structured JSON logging with redaction for `authorization`, `password`, `token`, and cookies
- [ ] OWASP ZAP / Burp Suite scan with all medium/high findings resolved

### Milestone 3 — Hardened Container & DevSecOps CI Pipeline
- [ ] Multi-stage `Dockerfile` (minimal Node LTS, non-root user, read-only root FS compatibility)
- [ ] GitHub Actions CI: `tsc --noEmit` + tests, Semgrep, Trivy, Gitleaks, Checkov

### Milestone 4 — Defensive GCP Deployment via Terraform
- [ ] Workload Identity Federation (GitHub Actions -> GCP, zero static SA keys)
- [ ] Least-privilege IAM for Cloud Run
- [ ] Private Cloud SQL (PostgreSQL, private IP only) + Cloud Storage (uniform access, public access prevention)
- [ ] Secret Manager runtime injection into Cloud Run
- [ ] External HTTPS Load Balancer + Cloud Armor (WAF) with Cloud Run ingress restricted to internal LB

### Milestone 5 — Detection, Auditability & Break/Fix Practice
- [ ] Cloud Audit Logs routed via Log Sink to BigQuery + 3 detection queries
- [ ] Break/Fix sandbox (GCPGoat / Thunder CTF) with 3 documented exploits and defensive Terraform fixes

## Primary References
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/)
- [NestJS Documentation](https://docs.nestjs.com/)
- [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [Google Cloud Documentation](https://cloud.google.com/docs)
- [Terraform Google Provider Docs](https://registry.terraform.io/providers/hashicorp/google/latest/docs)
