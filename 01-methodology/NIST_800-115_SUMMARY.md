# NIST SP 800-115 — ZEN0SEC Summary

**Full Title:** Technical Guide to Information Security Testing and Assessment  
**Published by:** NIST (National Institute of Standards and Technology)  
**Relevance:** Provides the authoritative US federal framework for security testing methodology

---

## Why NIST SP 800-115 Matters

NIST SP 800-115 is the foundational document that legitimizes security testing as a formal discipline. It defines:
- What security testing is and why it must be authorized
- The phases that every professional engagement must follow
- The legal and policy context that separates authorized testing from unauthorized access
- Post-execution responsibilities (reporting, remediation tracking)

---

## The Four Phases

### 1. Planning
- Define objectives, scope, and constraints
- Identify authorization chain
- Select appropriate techniques (review, identification, target validation)
- Establish rules of engagement

### 2. Discovery
**2a. Network/System Discovery**
- Identify active hosts, open ports, running services
- Tools: `nmap`, `masscan`, `netdiscover`

**2b. Vulnerability Discovery**
- Map services to known vulnerabilities
- Tools: `nessus`, `openvas`, `lynis`, `searchsploit`

### 3. Attack (Exploitation)
- Validate that discovered vulnerabilities are actually exploitable
- Controlled, scoped, documented
- Gain access, escalate privileges, pivot (within scope)
- All actions logged with timestamps

### 4. Post-Execution
- Clean up artifacts (persistence mechanisms, created accounts, uploaded files)
- Document findings in structured report
- Provide remediation recommendations
- Conduct lessons-learned review

---

## Key NIST Testing Techniques

| Technique | Description |
|---|---|
| **Network Scanning** | Host discovery, port scanning, service identification |
| **Vulnerability Scanning** | Automated identification of known vulnerabilities |
| **Password Cracking** | Offline hash cracking, brute force (authorized targets) |
| **Penetration Testing** | Active exploitation of identified vulnerabilities |
| **Social Engineering Testing** | Phishing, vishing (requires explicit authorization) |
| **Physical Security Testing** | Badge cloning, tailgating (requires explicit authorization) |

---

## NIST Reporting Requirements

- All findings documented with evidence
- Risk ratings assigned (use CVSS where possible)
- Remediation recommendations provided for each finding
- Report delivered to authorizing official
- Sensitive findings handled with appropriate data protection

---

## Reference

- Official PDF: https://csrc.nist.gov/publications/detail/sp/800/115/final
