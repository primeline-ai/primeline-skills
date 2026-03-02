# Security Review Checklist

The highest-value part of any code review. One caught vulnerability is worth a hundred style suggestions.

## The Top 10 Checks

Run through these for every review. They cover 90% of real-world vulnerabilities.

### 1. Injection

**Check:** Does user input flow into SQL, shell commands, HTML, or templates without sanitization?

```
Red flags:
- String concatenation in SQL queries instead of parameterized queries
- Template literals used to build database queries dynamically
- User input passed to shell execution functions
- User content rendered as raw HTML
```

**Fix:** Parameterized queries, shell escaping libraries, text content methods instead of raw HTML insertion.

### 2. Authentication

**Check:** Do new endpoints require authentication? Are there auth bypasses?

```
Red flags:
- New API route without auth middleware
- Auth check in the handler but not the middleware (can be skipped)
- JWT validation that doesn't check expiry
- Password comparison using == instead of timing-safe compare
```

**Fix:** Auth at the middleware level, not the handler. Check expiry. Use timing-safe comparison.

### 3. Authorization

**Check:** Can users access resources they shouldn't? Can regular users do admin things?

```
Red flags:
- No ownership check: user A can edit user B's data
- Role check missing: any authenticated user can access admin routes
- IDOR: changing an ID in the URL gives access to other records
- Privilege escalation: user can set their own role
```

**Fix:** Check ownership on every data access. Verify roles. Never trust client-sent role data.

### 4. Secrets in Code

**Check:** Are API keys, passwords, tokens, or connection strings committed to the repo?

```
Red flags:
- Hardcoded strings matching API key patterns (sk_live_, AKIA, ghp_)
- .env files added to git (should be in .gitignore)
- Config files with real credentials
- Comments containing passwords
```

**Fix:** Environment variables. .env in .gitignore. Secrets manager for production.

### 5. Input Validation

**Check:** Is user input validated before use?

```
Red flags:
- File upload without type/size check
- Email field accepts any string
- Numeric field not checked for negative/overflow
- JSON body not validated against schema
- Path parameter used in file system operations
```

**Fix:** Validate at the boundary. Type, format, range, size. Reject invalid input early.

### 6. Error Handling

**Check:** Do error responses leak internal information?

```
Red flags:
- Stack traces in API responses
- Database error messages shown to users
- Internal file paths in error output
- Verbose error in production, missing error in development
```

**Fix:** Generic error messages to users. Detailed logs to your monitoring system. Never expose internals.

### 7. SSRF (Server-Side Request Forgery)

**Check:** Can users make the server fetch arbitrary URLs?

```
Red flags:
- User-provided URL fetched server-side without validation
- Webhook URLs not validated against allow list
- Image proxy that accepts any URL
- Internal services reachable via user-controlled requests
```

**Fix:** URL allow lists. Block internal IP ranges (10.x, 172.16.x, 192.168.x, 127.x). Validate schemes (https only).

### 8. Cross-Site Scripting (XSS)

**Check:** Can user input appear in HTML output unescaped?

```
Red flags:
- React's raw HTML insertion used with user-provided content
- Template engines with unescaped output filters
- User-controlled URL attributes (href, src)
- JSON embedded in script tags without escaping
```

**Fix:** Use framework's auto-escaping. Sanitize with DOMPurify when rich text is needed. Add CSP headers.

### 9. Dependency Security

**Check:** Do new dependencies have known vulnerabilities?

```
Red flags:
- New dependency added without checking for CVEs
- Pinned to vulnerable version
- Abandoned package (no updates in 2+ years)
- Package with very few downloads (supply chain risk)
```

**Fix:** Run audit tools (npm audit, pip audit) before merging. Check package health.

### 10. Data Exposure

**Check:** Does the API return more data than the client needs?

```
Red flags:
- User endpoint returns password hash
- List endpoint returns all fields including internal ones
- Error includes other users' data
- Logs contain PII (email, phone, IP without consent)
```

**Fix:** Explicit field selection. Never send raw database objects. Audit log output.

## CI/CD Security (Bonus)

If the PR modifies CI/CD configuration:

```
Red flags:
- GitHub Actions: pull_request_target with checkout of PR code
- Secrets accessible to fork PRs
- Third-party actions not pinned to specific commit SHA
- Workflow permissions broader than needed
- Scripts downloaded and piped to shell without verification
```

**Fix:** Use pull_request event, not pull_request_target. Pin actions to SHA. Minimal permissions.

## Review Speed Guide

| PR Size | Focus On | Time Budget |
|---------|----------|-------------|
| < 100 lines | All 10 checks | 5-10 minutes |
| 100-500 lines | Top 5 checks + domain-specific | 15-30 minutes |
| 500+ lines | Security checks only, request split | 30-60 minutes |

For PRs over 500 lines, consider asking the author to split it. Large PRs hide bugs.
