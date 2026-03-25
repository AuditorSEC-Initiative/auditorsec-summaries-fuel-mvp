# Contributing to AuditorSEC Summaries & Fuel MVP

Thank you for contributing to this dual-purpose project combining Web3 audit summaries (FastAPI + LLM) and fleet fuel analytics (PostgreSQL + anomaly detection).

## How to Contribute

### 1. Setup
```bash
git clone https://github.com/AuditorSEC-Initiative/auditorsec-summaries-fuel-mvp
cd auditorsec-summaries-fuel-mvp
pip install -r requirements.txt  # or see pyproject.toml
```

### 2. Development Areas
- **Summaries Engine**: FastAPI endpoints, LLM prompt engineering, audit report generation
- **Fuel Analytics**: Trip reconstruction, anomaly detection algorithms, PostgreSQL optimization
- **Kubernetes**: Helm charts, HPA configs, service mesh integration
- **Documentation**: API docs, architecture diagrams, deployment guides

### 3. Reporting Issues
- Use issue templates (Bug Report / Feature Request)
- Include stack traces and reproduction steps
- Label issues appropriately (`bug`, `enhancement`, `fuel-analytics`, `summaries`)

### 4. Pull Requests
- Fork and create a feature branch
- Follow PEP 8 for Python code
- Add tests for new features
- Update documentation
- CI must pass before merge

### 5. Data Privacy
- Never commit real fuel/GPS data or customer information
- Use anonymized test datasets only
- Follow GDPR requirements for any EU data processing

## Code of Conduct
Be respectful and constructive. See our Code of Conduct.

## Contact
- GitHub Issues for bugs/features
- security@auditorsec.com for security issues
- Telegram: @AuditorSEC_Initiative
