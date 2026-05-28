# ZEN0SEC — Master Operational Playbook

**Operator:** Shaun Patrik Dyess / ZEN0SEC  
**Version:** 1.0 — 2026-05-27  
**Classification:** Internal — Lab Use Only

---

## Purpose

This is the single source of truth for all ZEN0SEC lab operations, research workflows, and future authorized engagements. Every exercise begins here.

---

## Pre-Operation Checklist (Every Time)

```
[ ] Ethics charter reviewed: 00-HQ/ETHICS_CHARTER.md
[ ] Scope document written or lab scenario defined
[ ] ROE completed if external engagement: 01-methodology/RULES_OF_ENGAGEMENT_TEMPLATE.md
[ ] Lab isolated (no internet route from lab VMs)
[ ] All target VMs snapshotted
[ ] Terminal logging enabled (script -a ~/logs/session-YYYYMMDD-HHMMSS.log)
[ ] SIEM/log capture running on Segment D
[ ] Exercise objectives written with PTES phase tags
```

---

## Capability Lanes Quick Reference

| Lane | Directory | Primary Tools | Key PDFs |
|---|---|---|---|
| **Networking** | `03-networking/` | nmap, Wireshark, tshark, masscan | Network_101, Network_Essentials, Wireshark_Cheat_Sheet |
| **Active Directory** | `04-active-directory/` | BloodHound, CrackMapExec, Impacket, mimikatz | AD_Attacks, AD_Post_Exploitation, Active_Directory_Pentest_Course, Attacking_AD_with_Linux |
| **Linux** | `05-linux/` | LinPEAS, pspy, GTFOBins, sudo exploits | Linux_Pentest, Linux_Privilege_Escalation |
| **Web & API** | `06-web-and-api/` | Burp Suite, sqlmap, ffuf, jwt_tool | SQLi, Web_Attacks, IDOR_Guide, JWT_Hacking, WAF, Burp_Suite |
| **Wireless & SSH** | `07-wireless-and-ssh/` | aircrack-ng, hashcat, ssh-audit | Wi_Fi_hacking, SSH_Access, Ssh_pen_testing |
| **Hardware/Memory** | `08-hardware-and-memory/` | rowhammer-test, custom Python | GPU_rowhammer, TRR_rowhammer, Blacksmith, Rowhammer_survey, Solving_rowhammer |
| **SOC/IR** | `09-soc-and-ir/` | Graylog/ELK, Sigma rules, Velociraptor | SOC_IR_PLAYBOOK |
| **Certs & Training** | `10-certifications-and-training/` | HTB, TryHackMe, OSCP labs | OSCP_Cheat_Sheet, Pentest_Guide, Pentest_Check_List, Bug_Bounty, Beginner_Pentesting |
| **Tools & Refs** | `11-tools-and-references/` | LoLBAS, Docker, DDoS tools | LoLbas1pg, Docker_pen_test, DDoS_Attack, curl_cheatsheet |

---

## Standard Lab Exercise Flow

### 1. Set Objective
```
Objective: [What are you practicing?]
PTES Phase: [Recon / Scanning / Exploitation / Post-Exploitation / Reporting]
OWASP WSTG ID: [If web: e.g., WSTG-INPV-05]
Target Segment: [A / B / C / D]
Target VM: [VM name and IP]
Time Box: [e.g., 2 hours]
```

### 2. Recon & Enumeration
```bash
# Network discovery
nmap -sn 10.10.20.0/24

# Full port scan
nmap -sV -sC -p- -oA scans/target-full 10.10.20.10

# Web tech fingerprinting (if web target)
whatweb http://10.10.30.10
nikto -h http://10.10.30.10
```

### 3. Vulnerability Analysis
```bash
# Search for exploits
searchsploit [service name and version]

# Run automated scanner (lab targets only)
nessus / openvas / nikto
```

### 4. Exploitation
```bash
# Document every command run
# Format: [timestamp] [command] [result]

# Example: Metasploit
msfconsole -q
use [module]
set RHOSTS 10.10.20.10
set LHOST 10.10.10.10
run
```

### 5. Post-Exploitation
```bash
# Enumerate post-access
id && whoami && hostname
uname -a
cat /etc/passwd
sudo -l

# Privesc check
./linpeas.sh | tee /tmp/linpeas-out.txt
```

### 6. Report & Purple Team
```
[ ] Write exercise report (template: PLAYBOOK/EXERCISE_REPORT_TEMPLATE.md)
[ ] Tag each finding with PTES phase and CVSS score
[ ] Purple team: what log events were generated?
[ ] What Sigma/detection rules would catch this?
[ ] Add to SIEM ruleset if gap identified
```

---

## Engagement Flow (Future Client Work)

1. Sign NDA + engagement agreement
2. Complete ROE: `01-methodology/RULES_OF_ENGAGEMENT_TEMPLATE.md`
3. Follow full NIST SP 800-115 + PTES methodology: `01-methodology/METHODOLOGY_ALIGNED.md`
4. Deliver report per NIST post-execution requirements
5. Conduct remediation retest if in scope

---

## Emergency Stop Protocol

If any of the following occur, **immediately stop all testing:**

1. Unintended access to out-of-scope system
2. Real user data encountered
3. Lab traffic detected on home LAN
4. Authorization revoked verbally or in writing

**Recovery:**
1. Disconnect lab VM network adapters
2. Document exactly what occurred with timestamps
3. Notify authorizing party if external engagement
4. Do NOT delete logs or evidence
