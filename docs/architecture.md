# AuditorSEC Summaries & Fuel MVP - Architecture

## Overview

Dual-purpose platform combining:
1. **Web3 Audit Summaries Engine** - FastAPI backend generating LLM-powered audit narrative reports
2. **Fleet Fuel Analytics MVP** - Anomaly detection, trip reconstruction, and fuel consumption analysis

## System Architecture

```
+----------------------------------+
|         API Gateway              |
|   (FastAPI + JWT Auth)           |
+----------------------------------+
         |              |
+---------------+  +------------------+
| Summaries     |  | Fuel Analytics   |
| Engine        |  | Engine           |
| (LLM/OpenAI)  |  | (Anomaly Det.)   |
+---------------+  +------------------+
         |              |
+----------------------------------+
|      PostgreSQL 15               |
|  (audit_reports | fuel_trips)    |
+----------------------------------+
         |
+----------------------------------+
|   Kubernetes / Helm              |
|   (HPA + Pod Autoscaling)        |
+----------------------------------+
```

## Core Components

### 1. Summaries Engine
- **FastAPI** REST endpoints for report generation
- **LLM Integration**: OpenAI GPT-4 / local Ollama fallback
- **Prompt Templates**: Smart contract audit → narrative conversion
- **Output formats**: Markdown, PDF (via WeasyPrint), JSON
- **Caching**: Redis for repeated audit patterns

### 2. Fuel Analytics MVP
- **Trip Reconstruction**: GPS + CAN bus data → trip segments
- **Anomaly Detection**: Statistical + ML-based fuel theft detection
- **Metrics**: L/100km, idle time, route efficiency scores
- **Alerts**: Telegram + email notifications for anomalies

### 3. Data Layer (PostgreSQL)
- `audit_reports` table: Web3 contract audit results
- `fuel_trips` table: Trip records with GPS coordinates
- `fuel_anomalies` table: Detected anomaly events
- `vehicles` table: Fleet registry

### 4. Kubernetes Deployment
- Namespace: `auditorsec-prod`
- HPA: 2-10 replicas based on CPU/memory
- PersistentVolumeClaim for PostgreSQL
- Ingress: nginx with TLS termination
- Secrets: Kubernetes secrets for API keys

## Data Flow

### Audit Summaries
```
SmartContract JSON → API endpoint → LLM prompt → 
Narrative report → PostgreSQL storage → PDF export
```

### Fuel Analytics
```
GPS/CAN data → Ingest API → Trip reconstruction → 
Anomaly detection → Alert → Dashboard
```

## Security
- JWT authentication on all endpoints
- GDPR: PII anonymization for GPS data
- Encrypted PostgreSQL connections (TLS)
- API rate limiting (100 req/min per client)
- Trivy scanning in CI/CD pipeline

## Tech Stack
- Python 3.11 + FastAPI
- PostgreSQL 15
- Redis (caching)
- Kubernetes + Helm
- GitHub Actions CI/CD
- OpenAI API / Ollama (local LLM)
