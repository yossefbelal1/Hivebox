# HiveBox Engineering Roadmap Checklist

End-to-end DevOps and SRE implementation checklist aligned with the Dynamic DevOps Roadmap (`devopsroadmap.io/projects/hivebox/`).

---

## Phase 1: Welcome & Agile Project Setup

- [x] Initialize GitHub repository (`yossefbelal1/Hivebox`) with branching and Pull Request workflow
- [x] Create GitHub Projects Kanban board (Project #2) with `Todo`, `In Progress`, and `Done` columns
- [x] Document initial project overview, architecture, and target openSenseMap `senseBox` IDs (`5eba5fbad46fb8001b799786`, `5c21ff8f919bf8001adf2488`, `5ade1acf223bd80019a1011c`) in `README.md`

---

## Phase 2: Basics - DevOps Core & Container MVP

- [x] Verify RHEL VM and Docker runtime environment with Bash health check script
- [x] Implement initial Python application (`app.py`) adopting Semantic Versioning (`v0.0.1`) that prints version and exits
- [x] Create initial `Dockerfile`, build `hivebox:v0.0.1` container image, and verify execution locally
- [x] Document local Docker build and run instructions in `README.md` and merge via Pull Request to `main`

---

## Phase 3: Start - Laying the Base

### Code & API
- [ ] Adopt Conventional Commits for all Git commit messages
- [ ] Implement openSenseMap API client module using Python (`requests`, dicts, functions, JSON parsing, error handling)
- [ ] Build HiveBox REST API using FastAPI or Flask exposing `GET /version` and `GET /temperature`
- [ ] Filter `/temperature` measurements to compute average temperature strictly from readings no older than 1 hour
- [ ] Write unit tests (`pytest` with mocked HTTP responses) covering `/version`, `/temperature`, and freshness filtering

### Containers & Tooling
- [ ] Configure Pylint (`.pylintrc` / `pyproject.toml`) and Hadolint (`.hadolint.yaml`) with VS Code extensions
- [ ] Refactor `Dockerfile` applying Docker best practices (pinned base image, non-root user, `.dockerignore`, pinned `requirements.txt`)

### Continuous Integration & Security
- [ ] Create GitHub Actions CI workflow (`.github/workflows/ci.yml`) running Pylint, Hadolint, unit tests, and Docker image build
- [ ] Spin up built container in CI and verify `GET /version` returns the expected version
- [ ] Integrate OpenSSF Scorecard GitHub Action and resolve reported repository findings

---

## Phase 4: Expand - Constructing a Shell

### Code & Testing
- [ ] Externalize target `senseBox` IDs to environment variables
- [ ] Extend `GET /temperature` response with temperature status thresholds: `< 10` (`Too Cold`), `11-36` (`Good`), `> 37` (`Too Hot`)
- [ ] Implement `GET /metrics` exposing default Prometheus application metrics
- [ ] Write API integration tests covering `/version`, `/temperature`, and `/metrics`

### Kubernetes & Local Cluster
- [ ] Configure local `kind` cluster with `ingress-nginx` enabled (ports 80/443)
- [ ] Create core Kubernetes manifests (`Deployment`, `Service`, `Ingress`, `ConfigMap`) and verify deployment with `kubectl`

### CI/CD & Quality Gates
- [ ] Extend GitHub Actions CI workflow to execute integration tests
- [ ] Integrate SonarQube (with Quality Gate check) and/or Semgrep static analysis in CI
- [ ] Create GitHub Actions CD workflow (`.github/workflows/cd.yml`) to build, tag, and push container images to registry

---

## Phase 5: Transform - Finishing the Structure

### Caching, Object Storage & Probes
- [ ] Integrate Valkey (Redis-compatible) caching layer with 5-minute refresh cycle and `/cache` endpoint
- [ ] Integrate MinIO (S3-compatible) object storage layer with 5-minute backup cycle and `/store` endpoint
- [ ] Implement `GET /readyz` readiness probe returning HTTP 200 unless `50% + 1` senseBoxes are inaccessible, Valkey cache is older than `2 cycles`, or MinIO backup is older than `2 cycles`
- [ ] Extend `/metrics` with custom application Prometheus metrics (cache status, storage status, senseBox availability)

### Packaging & Kubernetes Security
- [ ] Create HiveBox Helm chart (`charts/hivebox/`) with `readinessProbe` wired to `/readyz`
- [ ] Apply Kubernetes Security Best Practices (CKS-aligned `securityContext`, `readOnlyRootFilesystem`, dropped capabilities, resource limits, NetworkPolicies)
- [ ] Manage stateful infrastructure manifests (Valkey, MinIO) via Kustomize overlays

### IaC, Observability & End-to-End Testing
- [ ] Provision Kubernetes cluster via Terraform IaC (cloud free-tier or local emulator such as Floci/Kind)
- [ ] Deploy Grafana Agent to collect metrics and logs and ship them to Grafana Cloud / Loki
- [ ] Create declarative Venom E2E test suite (`tests/e2e/`) and execute it against a live Kind cluster in GitHub Actions CI

---

## Phase 6: Optimize - Keep Improving

- [ ] Implement declarative GitOps continuous delivery with Argo CD (`Application` / `ApplicationSet`) across `dev`, `stage`, and `prod`
- [ ] Configure `cert-manager` for automated TLS certificates and `external-dns` for automated Ingress DNS management
- [ ] Migrate external SaaS tools/dependencies to in-cluster open-source services where applicable
- [ ] Configure `.github/dependabot.yml` for automated dependency updates (`pip`, `docker`, `terraform`, `helm`, `github-actions`)
- [ ] Enforce Kubernetes Policy-as-Code rules using Kyverno

---

## Phase 7: Capstone Project

- [ ] Define an independent application idea and technology stack outside HiveBox
- [ ] Apply the complete DevOps lifecycle end-to-end: Agile Kanban planning, application & test suites (unit, integration, E2E), container hardening, Terraform IaC, Helm/Kustomize, CI/CD security gates, Argo CD GitOps, and full observability
