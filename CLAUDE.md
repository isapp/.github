# Review rubric (Claude)
Scope: Python (Django), Kotlin (Android), Swift (iOS), JavaScript/TypeScript (Vue 3), SQL/Redshift, Terraform/IaC.

## Must-fix

### Security
- OWASP Top 10 issues: SQL injection, XSS, CSRF, broken authentication, insecure deserialization
- AuthN/Z: verify JWT validation, session management, proper OAuth flows, role-based access
- API security: rate limiting, input validation, API key rotation, proper CORS configuration
- Secrets: no hardcoded credentials, use environment variables or AWS Secrets Manager
- PII handling: encrypt PII at rest and in transit, redact PII in logs and error messages
- FedRAMP/HIPAA compliance requirements

### Compliance (HIPAA/SOC2/FedRAMP)
- Logging: no PHI/PII in logs, CloudWatch logs encrypted, proper retention policies
- Encryption: use approved libraries (AES-256, TLS 1.2+), KMS for key management
- Access control: least-privilege IAM, MFA for admin access, proper role separation
- Audit trails: log access to sensitive data, retain audit logs per compliance requirements
- Data deletion: implement proper data lifecycle, support user data deletion requests

### Data & Migrations
- Migrations: safe, rollbackable, tested in staging before production
- Schema changes: backward compatible, no breaking changes without version bump
- Indexes: add indexes for foreign keys and frequently queried columns
- Data validation: constraints at database level, not just application level
- PHI/PII: ensure proper encryption columns, audit trails for sensitive data access

### Mobile (iOS/Android)
- UI thread safety: no network/database calls on main thread
- Background work: proper WorkManager (Android) or BackgroundTask (iOS) usage
- Permissions: clear rationale strings, runtime permission checks
- Network: timeouts, retries with exponential backoff, offline handling
- Security: certificate pinning, secure storage (Keychain/KeyStore), code obfuscation
- Accessibility: VoiceOver/TalkBack support, proper content descriptions

### Infrastructure (Terraform/IaC/Docker)
- IaC: Terraform state encryption, no hardcoded credentials, least-privilege IAM policies
- Container security: non-root users, minimal base images, scan for vulnerabilities
- Secrets: use AWS Secrets Manager/SSM Parameter Store, never commit secrets
- Network: proper security groups, VPC configuration, no public exposure of sensitive resources
- Monitoring: CloudWatch logs and alarms for critical resources

## Should-fix

### Performance
- Database: N+1 queries, missing indexes, unbounded result sets
- API: missing pagination for list endpoints, slow queries without caching
- Memory: large allocations, memory leaks, inefficient data structures
- Mobile: large image loading, excessive re-renders, battery drain

### Testing
- Unit tests: cover edge cases, error conditions, security validations
- Integration tests: test API contracts, database interactions, mock external services
- Security tests: authentication failures, authorization boundaries, input validation
- Test naming: use descriptive names (e.g., `test_create_user_with_invalid_email_returns_400`)
- Flag missing tests for critical business logic or compliance-related code

### APIs/Services
- API design: RESTful conventions, proper HTTP status codes, versioning strategy
- Error handling: consistent error response format, proper logging without PII
- Service-to-service: proper authentication tokens, circuit breakers, timeouts
- Pagination: implement for list endpoints to prevent large responses
- Documentation: ensure API changes include updated Swagger/OpenAPI docs

### Dependencies
- Security: flag outdated dependencies with known CVEs (check npm audit, pip-audit, etc.)
- Version pinning: use specific versions in production dependencies
- Licensing: ensure licenses are compatible with your usage
- Deprecations: flag usage of deprecated APIs or libraries

## Nice-to-have (optional suggestions)

- Code clarity: simplify complex logic, extract helper functions, add explanatory comments
- Documentation: update README for significant features, add inline docs for complex logic
- Performance optimizations: suggest caching opportunities, query optimizations (non-critical)
- Modern practices: suggest newer language features when beneficial (type hints, modern syntax)

## Style

- Follow repo linters/formatters (Black, ESLint, ktlint, SwiftLint)
- Keep suggestions concise and actionable
- Show patch-style diffs for proposed changes
- Group related issues together

## Behavior

- Be terse and direct in comments
- Prefer inline comments on specific diff lines when possible
- Prioritize must-fix issues over nice-to-have suggestions
- Skip review for:
  - Pure whitespace/formatting changes (if linters pass)
  - Documentation-only updates (unless security/compliance docs)
  - Configuration changes without code impact
  - Minor dependency bumps (unless security-related)
- If skipping, comment: "No significant code changes to review" and explain why
