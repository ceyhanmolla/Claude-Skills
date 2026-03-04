⚠️ REFERANS: Bu dosya MASTER_INSTRUCTIONS.md protokollerine tabidir. Tüm güncellemeler ve analizler Master dosyadaki direktiflere göre yapılmalıdır.

# SECURITY.MD

## 1. Security Audit Summary

* **Assessed By:** Senior Security Researcher (AI)
* **Overall Risk Assessment:** [AI: Critical / High / Medium / Low / Secure]
* **Scope of Audit:** [AI: e.g., API Endpoints, Auth Logic, Data Encryption, Dependency Vulnerabilities]
* **Top Critical Risks:** [AI: List the most dangerous findings first]

---

## 2. Vulnerability Findings & Exploit Scenarios

*This section is dynamically updated by the AI after a full code review.*

### [AI: Vulnerability Name - e.g., IDOR in Profile Update]

* **Severity:** [Critical / High / Medium / Low]
* **Location:** `[File Path & Line Number]`
* **The Exploit:** [AI: Technical description of how an attacker would abuse this. Step-by-step scenario.]
* **Technical Impact:** [e.g., Data breach, unauthorized access, privilege escalation]
* **The Fix:** ```[language]
// AI: Remediation code or instructions

```
* **Verification:** [How to test if the fix is successful]

---

## 3. Secret & Credential Audit
*Checklist for hardcoded sensitive information.*
* **API Keys / Secrets:** [AI: Status - Clear / Found]
* **Database Credentials:** [AI: Status]
* **Encryption Keys:** [AI: Status]
* **Environment Variables:** [AI: Are they properly configured?]

---

## 4. Security Hardening Recommendations
*General improvements to make the system more resilient.*
- [ ] [AI: Suggested hardening - e.g., Implement Rate Limiting]
- [ ] [AI: Suggested hardening - e.g., Add Security Headers (HSTS, CSP)]
- [ ] [AI: Suggested hardening - e.g., Sanitize all user inputs]

---

## 5. Dependency & Supply Chain Check
*Monitoring for outdated or insecure packages.*
* **Vulnerable Dependencies:** [AI: List any CVE-tagged libraries found]
* **Recommended Updates:** [AI: Versions to upgrade to]

---

## 6. Audit Logs & Sign-off
* **Last Audit Date:** [Timestamp]
* **Status:** [In Progress / Remediation Required / Verified Secure]

