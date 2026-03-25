# Security Policy

## Supported Versions

| Version | Supported |
| ------- | --------- |
| main    | Yes       |

## Reporting a Vulnerability

Do NOT open a public GitHub issue for security vulnerabilities.

### Contact
- **Email:** security@auditorsec.com
- **Response time:** Within 48 hours

### Process
1. Email with: description, steps to reproduce, impact
2. Acknowledgment within 48 hours
3. Fix timeline provided
4. Credit in release notes (if desired)
5. Public disclosure after patch

## Scope
- FastAPI endpoints and authentication (JWT)
- PostgreSQL access controls and data handling
- LLM API key management
- GPS/fuel data privacy (GDPR)
- Kubernetes cluster configurations
- Dependency vulnerabilities

## Data Privacy
This platform processes GPS location data and fuel telemetry. All data is handled in accordance with GDPR. Report any privacy violations to security@auditorsec.com.

## Out of Scope
- Third-party LLM provider vulnerabilities (report to OpenAI/Ollama)
- Physical server access
- Social engineering

Thank you for helping keep AuditorSEC Summaries & Fuel MVP secure.
