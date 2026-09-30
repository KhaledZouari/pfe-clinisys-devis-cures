# Security, Privacy, and Limitations

## Positioning

This portfolio does not claim GDPR compliance, compliance with Tunisian law, complete application security, or production deployment. Such claims require organizational and technical evidence that cannot be demonstrated through public portfolio documentation alone.

## Sensitive-data boundary

The workflow can involve identity and healthcare-related information. This data must be treated as sensitive. The public repository contains no real, pseudonymized, or production-derived patient record.

## Security responsibilities

| Layer | Expected responsibility |
| --- | --- |
| Browser | Minimize retained data, encode output, validate input for usability, and avoid sensitive logs |
| .NET API | Authenticate, authorize, validate, recalculate trusted amounts, and return safe errors |
| SQL Server | Enforce integrity, least privilege, secure connections, backup, and recovery controls |
| Deployment | Protect secrets, enforce HTTPS, monitor access, patch dependencies, and isolate environments |

## Known limitations and recommendations

| Priority | Limitation | Recommended treatment |
| --- | --- | --- |
| Critical | Multi-operation submission may produce partial results | Aggregate endpoint and server-side transaction |
| High | Sensitive context may temporarily exist in browser state | Transfer a minimal identifier and reload data after authorization |
| High | Private authorization rules cannot be audited publicly | Systematic API checks and role-based security tests |
| High | Validation evidence is incomplete | Structured client and server validation with contract tests |
| High | Automated coverage is not demonstrated in this portfolio | Unit, integration, API, and end-to-end tests |
| Medium | Legacy front-end dependencies require review | Inventory, vulnerability audit, and incremental upgrades |
| Medium | Error handling is partially coupled to pages | Standard error contract and privacy-safe logging |
| Review required | XSS, CSRF, injection, upload, and secret-management controls | Full-system security review |

## Publication safeguards

- No real or pseudonymized patient data
- No raw screenshot from an enterprise environment
- No internal URL, IP address, hostname, customer, or employee identity
- No token, credential, internal identifier, or private configuration excerpt
- Only reconstructed illustrations marked **FICTITIOUS DATA**
- Metadata review before an image is committed
- Manual confidentiality review before every publication

## Clinical boundary

The module prepares an administrative quotation and a projected schedule. It is not a prescription system, protocol validator, clinical decision-support tool, or adverse-event monitoring system. Any such evolution would require separate medical, regulatory, ethical, privacy, and security governance.

## Compliance disclaimer

Applicable law, data-controller responsibilities, retention policies, access governance, audit logging, incident response, backups, processor agreements, and impact assessments require validation by qualified stakeholders. This document is not legal or medical advice.
