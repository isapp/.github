# Review rubric (Claude)
Scope: Python (Django), Kotlin (Android), Swift (iOS), SQL/Redshift.

## Must-fix
- Security: OWASP issues, SQL injection, authN/Z, FedRamp compliance, HIPAA compliance, PII mishandling.
- Compliance: HIPAA/SOC2/FedRAMP basics (no PHI in logs; encryption libs; least-priv IAM; KMS usage).
- Data: migrations safe/rollbackable, schema changes backward compatible.
- Mobile: UI thread safety; background work; permissions rationale; network timeouts/retries.

## Should-fix
- Performance hot spots, N+1 queries, unbounded loops, large allocations.
- Testing gaps: add/adjust unit & integration tests; suggest test names.

## Style
- Follow repo linters/formatters; keep suggestions concise; show patch-style diffs.

## Behavior
- Be terse. Prefer inline comments on diffs.
- If low-signal changes (docs/whitespace), skip review.
