# HiveBox

An end-to-end DevOps & SRE hands-on project built iteratively following the Dynamic DevOps Roadmap. HiveBox is a RESTful API service for beekeepers that integrates with openSenseMap to fetch and serve environmental sensor data.

## Project Roadmap & Phases

- [x] **Phase 1:** Project Setup, Documentation & Agile Kanban Planning
- [x] **Phase 2:** Core Application MVP (`v0.0.1`) & Docker Containerization
- [ ] **Phase 3:** REST API Endpoints, Unit Testing, Linters & CI Pipeline (GitHub Actions)
- [ ] **Phase 4:** Local Kubernetes Orchestration (KIND), Prometheus Metrics & CD
- [ ] **Phase 5:** Caching (Valkey), Object Storage (MinIO), Helm & Terraform IaC
- [ ] **Phase 6:** Multi-Environment GitOps Delivery (Argo CD) & Observability

---

## Phase 1: Configuration & Sensor IDs

- **Methodology:** Kanban (tracked via GitHub Issues & Projects).
- **Git Strategy:** Feature branches merged via Pull Requests into `main`.
- **Target openSenseMap `senseBox IDs`:**
  - `5eba5fbad46fb8001b799786`
  - `5c21ff8f919bf8001adf2488`
  - `5ade1acf223bd80019a1011c`

---

## Phase 2: Running & Testing Locally (Docker)

The initial MVP (`v0.0.1`) prints the current application version and exits.

### 1. Build the Docker Image
```bash
docker build -t hivebox:v0.0.1 .
