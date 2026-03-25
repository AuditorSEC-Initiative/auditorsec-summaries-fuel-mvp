# AuditorSEC Summaries Engine & Fuel Analytics MVP

> **Two production-ready supporting services for the AuditorSEC security platform**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110-green)](https://fastapi.tiangolo.com)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue)](#)

---

## Overview

This repo contains two independent but complementary MVP services:

1. **Web3 Summaries Engine**: Generates structured LLM-powered narrative summaries from audit findings and protocol metadata.
2. **Fuel Analytics API**: Fleet management and fuel anomaly detection system with trip reconstruction and z-score anomaly scoring.

Both services run inside the AuditorSEC Kubernetes stack and integrate with the core orchestrator via NATS JetStream.

---

## Service 1: Web3 Summaries Engine

### What it does
- Accepts audit findings and protocol metadata via REST API.
- Generates structured human-readable summaries using LLM (meta.llmContext layer).
- Returns severity classification, remediation priorities, and executive summaries.
- Integrates with n8n workflows and Temporal for async report generation.

### API

```bash
POST /summaries/audit
Content-Type: application/json

{
  "protocol": "MyDeFiProtocol",
  "chain": "ethereum",
  "findings": [
    {"id": "REENT-001", "severity": "HIGH", "description": "Reentrancy in withdraw()"},
    {"id": "ACCESS-002", "severity": "MEDIUM", "description": "Missing onlyOwner modifier"}
  ]
}
```

Response:
```json
{
  "executive_summary": "Two critical vulnerabilities detected...",
  "remediation_priority": ["REENT-001", "ACCESS-002"],
  "risk_score": 8.4,
  "llm_context": {"model": "gpt-4o", "tokens_used": 512}
}
```

### Stack
- FastAPI + Pydantic v2
- LLM: OpenAI / Ollama / local model support
- Queue: NATS JetStream + Temporal worker
- Storage: PostgreSQL (audit_summaries table)

---

## Service 2: Fuel Analytics API

### What it does
- Manages fleet clients, vehicles, fuel stations, and transactions.
- Reconstructs trips from GPS + fuel event streams.
- Detects anomalies using z-scores and deviation thresholds.
- Provides REST endpoints for reporting and recomputing scores.

### Data Model

```
Client
  -> Vehicle (fleet assignment)
  -> FuelStation (location, price index)
  -> Transaction (volume, timestamp, GPS)
  -> Trip (reconstructed from transactions)
  -> AnomalyScore (z-score, deviation flag)
```

### API Examples

```bash
# Get all anomalous transactions for a client
GET /fuel/clients/{client_id}/anomalies?threshold=2.5

# Reconstruct trips for a vehicle
POST /fuel/vehicles/{vehicle_id}/reconstruct-trips

# Recompute anomaly scores
POST /fuel/clients/{client_id}/recompute-scores
```

### Stack
- FastAPI + SQLAlchemy 2.0 (async)
- PostgreSQL 15 with PostGIS for GPS queries
- Anomaly detection: scipy z-scores + custom deviation rules
- K8s: Deployment in `fuel-analytics` namespace with HPA

---

## Quick Start

### Prerequisites
- Python 3.11+
- PostgreSQL 15
- Docker (optional)

### Run Summaries Engine

```bash
cd summaries-engine
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8001
```

### Run Fuel Analytics API

```bash
cd fuel-analytics
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8002
```

### Run both with Docker Compose

```bash
docker compose up -d
# Summaries: http://localhost:8001/docs
# Fuel:      http://localhost:8002/docs
```

---

## Kubernetes Deployment

Both services have K8s manifests under `/k8s/`:

```
/k8s/
  summaries-engine/
    deployment.yaml
    service.yaml
    hpa.yaml
  fuel-analytics/
    deployment.yaml
    service.yaml
    hpa.yaml
  argocd-app.yaml
```

Deploy via ArgoCD:
```bash
kubectl apply -f k8s/argocd-app.yaml
```

---

## Observability

- Prometheus metrics exposed at `/metrics` on both services.
- Pre-built Grafana dashboards in `/dashboards/`.
- Namespaces: `auditorsec-core` (summaries) and `fuel-analytics` (fuel).
- Alerts: summary latency > 5s, anomaly rate spike > 20%.

---

## Roadmap

- [x] FastAPI skeleton with Pydantic v2 models.
- [x] PostgreSQL + SQLAlchemy async setup.
- [x] Z-score anomaly detection.
- [ ] Production hardening and multi-tenant support.
- [ ] Integration with core AuditorSEC LLM-Bridge orchestrator.
- [ ] Public demo datasets and Jupyter notebooks.
- [ ] RehabFund stack integration (impact fund reporting).
- [ ] Monday board automations for report scheduling.

---

## Related Repos

- [auditorsec-llm-bridge](https://github.com/AuditorSEC-Initiative/auditorsec-llm-bridge): Core orchestrator that consumes summaries.
- [bachmach-pqc-iot-sentinel](https://github.com/AuditorSEC-Initiative/bachmach-pqc-iot-sentinel): IoT telemetry feeds fuel analytics.
- [auditorsec-initiative-eu](https://github.com/AuditorSEC-Initiative/auditorsec-initiative-eu): Programme overview.

---

## Contact

- Email: romanchaa997@auditorsec.com
- Telegram: [@audityzerbot](https://t.me/audityzerbot)
- Notion workspace: [AuditorSEC Hub](https://www.notion.so/auditorsec)

---

## License

MIT License. See [LICENSE](LICENSE).
