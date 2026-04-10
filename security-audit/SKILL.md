---
name: security-audit
description: Security audit and vulnerability assessment skill for code review. Use when performing security audits, identifying vulnerabilities, checking for hardcoded secrets, reviewing authentication/authorization logic, analyzing dependency vulnerabilities, or providing security hardening recommendations. Covers OWASP Top 10, secret detection, credential auditing, and security best practices.
---

# Security Audit

## When to Use This Skill

Apply when performing security audits, vulnerability assessments, or security-focused code reviews. Use this skill when:

- ✅ Conducting security audits on codebases
- ✅ Identifying and documenting vulnerabilities
- ✅ Checking for hardcoded secrets and credentials
- ✅ Reviewing authentication and authorization logic
- ✅ Analyzing dependency vulnerabilities (CVEs)
- ✅ Providing security hardening recommendations
- ✅ Reviewing API security, data encryption, or input validation
- ✅ Assessing overall security posture and risk levels

**Context:** This skill provides a structured framework for security audits, including vulnerability documentation, exploit scenarios, remediation guidance, and security best practices.

## Security Audit Framework

### 1. Security Audit Summary

Document the overall assessment:

- **Assessed By:** Security Researcher / AI
- **Overall Risk Assessment:** Critical / High / Medium / Low / Secure
- **Scope of Audit:** API Endpoints, Auth Logic, Data Encryption, Dependency Vulnerabilities, etc.
- **Top Critical Risks:** List the most dangerous findings first

### 2. Vulnerability Findings & Exploit Scenarios

Document each vulnerability with:

- **Severity:** Critical / High / Medium / Low
- **Location:** File Path & Line Number
- **The Exploit:** Technical description of how an attacker would abuse this. Step-by-step scenario.
- **Technical Impact:** Data breach, unauthorized access, privilege escalation, etc.
- **The Fix:** Remediation code or instructions
- **Verification:** How to test if the fix is successful

**Example Vulnerability Documentation:**

```markdown
### IDOR in Profile Update

* **Severity:** Critical
* **Location:** `src/api/users/updateProfile.js:45`
* **The Exploit:** An attacker can modify the `userId` parameter in the request body to update another user's profile. The API does not verify that the requesting user owns the target profile.
* **Technical Impact:** Unauthorized data modification, account takeover
* **The Fix:**
```javascript
// Add authorization check
if (req.body.userId !== req.user.id) {
  return res.status(403).json({ error: 'Forbidden' });
}
```
* **Verification:** Attempt to update another user's profile; should return 403
```

### 3. Secret & Credential Audit

Checklist for hardcoded sensitive information:

- **API Keys / Secrets:** Clear / Found
- **Database Credentials:** Clear / Found
- **Encryption Keys:** Clear / Found
- **Environment Variables:** Are they properly configured?

**Best Practices:**
- Never commit secrets to version control
- Use environment variables or secret management services
- Rotate exposed credentials immediately
- Implement secret scanning in CI/CD pipelines

### 4. Security Hardening Recommendations

General improvements to make the system more resilient:

- [ ] Implement Rate Limiting
- [ ] Add Security Headers (HSTS, CSP, X-Frame-Options)
- [ ] Sanitize all user inputs
- [ ] Enable HTTPS everywhere
- [ ] Implement proper authentication (JWT, OAuth, etc.)
- [ ] Add logging and monitoring
- [ ] Implement CORS policies
- [ ] Use parameterized queries to prevent SQL injection
- [ ] Validate and sanitize file uploads
- [ ] Implement proper session management

### 5. Dependency & Supply Chain Check

Monitoring for outdated or insecure packages:

- **Vulnerable Dependencies:** List any CVE-tagged libraries found
- **Recommended Updates:** Versions to upgrade to

**Common Vulnerability Types:**
- OWASP Top 10 (Injection, Broken Auth, XSS, etc.)
- Known CVEs in dependencies
- Outdated packages with security patches
- Malicious packages in supply chain

**Tools to Use:**
- `npm audit` / `yarn audit` / `pnpm audit`
- `snyk test`
- `dependabot`
- GitHub Security Advisories

### 6. Audit Logs & Sign-off

- **Last Audit Date:** Timestamp
- **Status:** In Progress / Remediation Required / Verified Secure

## Common Vulnerability Patterns

### Injection Vulnerabilities

**SQL Injection:**
```javascript
// VULNERABLE
const query = `SELECT * FROM users WHERE id = '${userId}'`;

// SECURE
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [userId]);
```

**Command Injection:**
```javascript
// VULNERABLE
const output = require('child_process').execSync(`ls ${userInput}`);

// SECURE
const { execSync } = require('child_process');
const safeInput = require('shell-escape').escape([userInput]);
const output = execSync(`ls ${safeInput}`);
```

### Authentication & Authorization

**Broken Authentication:**
- Weak password policies
- Missing multi-factor authentication
- Session fixation
- Insecure password storage (no hashing/salting)

**Broken Authorization:**
- Missing access control checks
- IDOR (Insecure Direct Object References)
- Privilege escalation vulnerabilities
- Missing role-based access control

### Cross-Site Scripting (XSS)

```javascript
// VULNERABLE
div.innerHTML = userInput;

// SECURE
div.textContent = userInput;
// or use a sanitization library
import DOMPurify from 'dompurify';
div.innerHTML = DOMPurify.sanitize(userInput);
```

### Cross-Site Request Forgery (CSRF)

**Mitigation:**
- Implement CSRF tokens
- Use SameSite cookie attribute
- Verify Origin/Referer headers
- Use custom headers for state-changing requests

## Security Best Practices

### Input Validation
- Validate all user inputs on both client and server
- Use allow-lists (whitelisting) instead of block-lists
- Sanitize data before use
- Implement proper type checking

### Output Encoding
- Encode output based on context (HTML, JavaScript, URL, CSS)
- Use template engines with auto-escaping
- Never trust data from untrusted sources

### Cryptography
- Use standard, well-vetted cryptographic libraries
- Never implement your own cryptography
- Use strong algorithms (AES-256, RSA-2048+, SHA-256+)
- Proper key management and rotation

### Error Handling
- Don't expose sensitive information in error messages
- Log errors securely on the server
- Show generic error messages to users
- Implement proper exception handling

### Logging & Monitoring
- Log security-relevant events
- Monitor for suspicious activity
- Implement alerting for critical events
- Regularly review logs

## Do Not

- ❌ Commit secrets, API keys, or credentials to version control
- ❌ Use hardcoded credentials in code
- ❌ Trust user input without validation
- ❌ Use weak encryption or deprecated algorithms
- ❌ Ignore security warnings from dependency scanners
- ❌ Disable security features for convenience
- ❌ Expose sensitive information in error messages
- ❌ Use eval() or similar functions with user input
- ❌ Implement custom cryptography
- ❌ Skip security reviews for "temporary" code

## Severity Classification

**Critical:** Immediate action required. Exploitable with minimal effort, high impact.

**High:** Should be fixed soon. Exploitable with moderate effort, significant impact.

**Medium:** Should be addressed. Exploitable with significant effort, moderate impact.

**Low:** Should be fixed when possible. Minor security issues, low impact.

## Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Common Weakness Enumeration](https://cwe.mitre.org/)
- [CVE Database](https://cve.mitre.org/)
- [Snyk Vulnerability Database](https://snyk.io/vuln/)
