# ZEN0SEC — Unified Testing Methodology

**Standards:** NIST SP 800-115 | PTES | OWASP WSTG  
**Operator:** Shaun Patrik Dyess / ZEN0SEC  
**Scope:** All authorized lab exercises and future client engagements

---

## Overview

All ZEN0SEC testing activity is governed by a three-layer methodology stack:

| Layer | Standard | Role |
|---|---|---|
| **Policy & Compliance** | NIST SP 800-115 | Defines planning, execution, and post-execution phases; provides legal/policy anchor |
| **Technical Execution** | PTES | Defines the full kill-chain: pre-engagement → recon → threat modeling → exploitation → post-exploitation → reporting |
| **Web & API Testing** | OWASP WSTG | Detailed technical test cases for every web/API attack category |

---

## Phase 1: Pre-Engagement (NIST Planning + PTES Pre-Engagement)

### Required Before Any Testing Activity

- [ ] Written authorization obtained (signed ROE for engagements, lab scenario doc for internal exercises)
- [ ] Scope defined: target IPs/ranges, services, allowed techniques, excluded systems
- [ ] Rules of Engagement (ROE) documented — see `PLAYBOOK/ROE_TEMPLATE.md`
- [ ] Emergency contacts identified (for real engagements)
- [ ] Testing window confirmed
- [ ] Data handling rules established (no exfil of real data, even in testing)

### Lab Exercise Pre-Check
- [ ] VMs snapshotted before exercise begins
- [ ] Lab network confirmed isolated (no route to home LAN or internet)
- [ ] Logging/capture running on management segment
- [ ] Exercise scenario and objectives written down

---

## Phase 2: Intelligence Gathering / Reconnaissance

**PTES Phase:** Intelligence Gathering  
**NIST Phase:** Discovery

### Passive Recon
- OSINT: DNS, WHOIS, certificate transparency, LinkedIn/social (authorized scope only)
- Tools: `whois`, `dig`, `nslookup`, `theHarvester`, `recon-ng`, `shodan` (passive queries)
- ZEN0SEC resource: `03-networking/` PDFs

### Active Recon
- Network discovery: `nmap`, `masscan`, `netdiscover`
- Service enumeration: `nmap -sV -sC`, banner grabbing
- Web tech fingerprinting: `whatweb`, `wappalyzer`, `nikto`
- Tools: Contained to authorized lab subnet only

---

## Phase 3: Threat Modeling & Vulnerability Analysis

**PTES Phase:** Threat Modeling + Vulnerability Analysis  
**NIST Phase:** Vulnerability Validation

- Map discovered services to CVEs: `searchsploit`, NVD, Exploit-DB
- Host/config audit: `lynis`, `openvas`, OpenSCAP OVAL (Ubuntu)
- Web vulnerability scanning: `nikto`, `burpsuite` (spider + scanner)
- AD enumeration: `BloodHound`, `ldapdomaindump`, `enum4linux-ng`
- OWASP WSTG test cases: input validation, auth testing, session management

---

## Phase 4: Exploitation

**PTES Phase:** Exploitation  
**NIST Phase:** Exploitation (controlled, scoped)

### Rules
- Only against targets explicitly listed in the scope document
- Snapshots taken before any destructive technique
- All commands logged (terminal logging enabled)
- No production systems, no unauthorized external targets

### Execution Framework
- `metasploit` for known CVE exploitation (lab targets only)
- Custom Python/Bash exploits (see scripts lib)
- Web exploitation: manual + Burp Suite — mapped to OWASP WSTG test IDs
- AD exploitation: per `04-active-directory/` playbooks
- Linux privesc: per `05-linux/` playbooks

---

## Phase 5: Post-Exploitation

**PTES Phase:** Post Exploitation  
**NIST Phase:** Post Exploitation

- Establish persistence (lab only, documented, removed post-exercise)
- Lateral movement: pivot techniques per AD and Linux playbooks
- Credential harvesting: `mimikatz` (Windows lab), `secretsdump` (AD lab)
- Data discovery: identify what would be at risk in a real engagement
- Pivoting: SSH tunnels, port forwarding, SOCKS proxy

---

## Phase 6: Reporting

**PTES Phase:** Reporting  
**NIST Phase:** Post-Execution Activities

### Minimum Report Components
1. **Executive Summary** — scope, objectives, high-level findings
2. **Technical Findings** — each finding with: CVE/WSTG ID, severity, evidence, reproduction steps
3. **Risk Rating** — CVSS score or qualitative (Critical/High/Medium/Low/Info)
4. **Remediation Recommendations** — specific, actionable
5. **Appendices** — raw tool output, screenshots, logs

### Lab Exercise Report (simplified)
- Scenario description
- Techniques used (with PTES phase tag)
- What worked / what was blocked (detection)
- Purple team notes — how would this appear in logs?

---

## OWASP WSTG Test Category Index

| ID | Category | ZEN0SEC Resource |
|---|---|---|
| WSTG-INFO | Information Gathering | `03-networking/`, `06-web-and-api/` |
| WSTG-CONF | Configuration Testing | `11-tools-and-references/` |
| WSTG-IDNT | Identity Management | `06-web-and-api/` |
| WSTG-ATHN | Authentication Testing | `06-web-and-api/`, `07-wireless-and-ssh/` |
| WSTG-AUTHZ | Authorization Testing | `06-web-and-api/` (IDOR) |
| WSTG-SESS | Session Management | `06-web-and-api/` (JWT) |
| WSTG-INPV | Input Validation | `06-web-and-api/` (SQLi, Web Attacks) |
| WSTG-ERRH | Error Handling | `06-web-and-api/` |
| WSTG-CRYP | Cryptography | `07-wireless-and-ssh/` |
| WSTG-BUSL | Business Logic | `06-web-and-api/` |
| WSTG-CLNT | Client-Side Testing | `06-web-and-api/` |
| WSTG-APIT | API Testing | `06-web-and-api/` |

---

## References

- [NIST SP 800-115](https://csrc.nist.gov/publications/detail/sp/800/115/final)
- [PTES Technical Guidelines](http://www.pentest-standard.org/)
- [OWASP WSTG](https://owasp.org/www-project-web-security-testing-guide/)
