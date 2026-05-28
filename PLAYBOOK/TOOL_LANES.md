# ZEN0SEC — Tool Lanes Index

Every tool below maps to a PTES phase, NIST technique, and ZEN0SEC capability lane directory.

---

## Recon & Intelligence Gathering

| Tool | Purpose | PTES Phase | Install |
|---|---|---|---|
| `nmap` | Port scanning, service detection, OS fingerprinting | Recon | Pre-installed (Parrot) |
| `masscan` | Ultra-fast port scanning (lab use only) | Recon | `sudo apt install masscan` |
| `theHarvester` | OSINT email/domain harvesting | Intel Gathering | Pre-installed |
| `recon-ng` | Modular OSINT framework | Intel Gathering | Pre-installed |
| `whois` / `dig` | DNS/domain intelligence | Intel Gathering | Pre-installed |
| `whatweb` | Web technology fingerprinting | Intel Gathering | Pre-installed |
| `shodan` (CLI) | Passive internet-facing asset search | Intel Gathering | `pip install shodan` |
| `amass` | Subdomain enumeration | Intel Gathering | `sudo apt install amass` |

---

## Vulnerability Analysis

| Tool | Purpose | PTES Phase | Install |
|---|---|---|---|
| `nikto` | Web server vulnerability scanner | Vuln Analysis | Pre-installed |
| `lynis` | Linux host security audit | Vuln Analysis | `sudo apt install lynis` |
| `openvas` / `greenbone` | Network vulnerability scanner | Vuln Analysis | `sudo apt install openvas` |
| `searchsploit` | Exploit-DB offline search | Vuln Analysis | Pre-installed |
| `wpscan` | WordPress vulnerability scanner | Vuln Analysis | Pre-installed |
| `nuclei` | Template-based vulnerability scanner | Vuln Analysis | `go install` or binary |

---

## Exploitation

| Tool | Purpose | PTES Phase | Install |
|---|---|---|---|
| `metasploit` | Exploit framework | Exploitation | Pre-installed |
| `sqlmap` | Automated SQL injection | Exploitation | Pre-installed |
| `hydra` | Network service brute-force | Exploitation | Pre-installed |
| `john` / `hashcat` | Password cracking (offline) | Exploitation | Pre-installed |
| `burpsuite` | Web proxy / manual web exploitation | Exploitation | Pre-installed |
| `ffuf` | Web fuzzing (dirs, params, subdomains) | Exploitation | Pre-installed |
| `evil-winrm` | Windows Remote Management shell | Exploitation | `gem install evil-winrm` |
| `impacket` | Windows protocol exploitation suite | Exploitation | Pre-installed |

---

## Active Directory

| Tool | Purpose | PTES Phase | Install |
|---|---|---|---|
| `BloodHound` + `neo4j` | AD attack path visualization | Recon + Post-Exploit | `sudo apt install bloodhound` |
| `CrackMapExec` (CME) | AD lateral movement and spraying | Exploitation | `sudo apt install crackmapexec` |
| `ldapdomaindump` | LDAP AD enumeration | Recon | `pip install ldapdomaindump` |
| `enum4linux-ng` | SMB/AD enumeration | Recon | `sudo apt install enum4linux-ng` |
| `responder` | LLMNR/NBT-NS poisoning | Exploitation | Pre-installed |
| `mimikatz` | Windows credential extraction | Post-Exploitation | Windows-side tool |
| `secretsdump` (Impacket) | Remote credential dumping | Post-Exploitation | Impacket suite |
| `kerbrute` | Kerberos user enumeration + brute-force | Recon | Binary release |

---

## Web & API

| Tool | Purpose | PTES Phase | WSTG Mapping |
|---|---|---|---|
| `burpsuite` | Intercepting proxy, scanner, repeater | Exploitation | All WSTG categories |
| `sqlmap` | SQL injection automation | Exploitation | WSTG-INPV-05 |
| `ffuf` | Directory/parameter fuzzing | Recon | WSTG-INFO |
| `jwt_tool` | JWT attack toolkit | Exploitation | WSTG-SESS |
| `wfuzz` | Web fuzzer | Exploitation | WSTG-INPV |
| `dalfox` | XSS scanner | Exploitation | WSTG-INPV-01 |
| `arjun` | HTTP parameter discovery | Recon | WSTG-INFO |

---

## Wireless & SSH

| Tool | Purpose | PTES Phase | Install |
|---|---|---|---|
| `aircrack-ng` | Wi-Fi capture and cracking | Exploitation | Pre-installed |
| `airmon-ng` | Monitor mode management | Recon | Pre-installed |
| `hcxdumptool` | PMKID/handshake capture | Recon | `sudo apt install hcxdumptool` |
| `hcxtools` | Convert captures for hashcat | Post-capture | `sudo apt install hcxtools` |
| `hashcat` | GPU password cracking | Exploitation | Pre-installed |
| `ssh-audit` | SSH server configuration audit | Vuln Analysis | `sudo apt install ssh-audit` |
| `hydra` | SSH brute-force | Exploitation | Pre-installed |

---

## Post-Exploitation & Pivoting

| Tool | Purpose | PTES Phase | Install |
|---|---|---|---|
| `linpeas` / `winpeas` | Privilege escalation enum | Post-Exploitation | Download from GitHub |
| `pspy` | Process monitoring (no root) | Post-Exploitation | Download binary |
| `chisel` | Fast TCP tunnel over HTTP | Post-Exploitation | Download binary |
| `ligolo-ng` | Tunneling and pivoting | Post-Exploitation | Download binary |
| `proxychains` | Route tools through SOCKS proxy | Post-Exploitation | Pre-installed |
| `socat` | Port forwarding and relay | Post-Exploitation | Pre-installed |

---

## SOC / IR / Detection

| Tool | Purpose | Lane |
|---|---|---|
| `Graylog` | Centralized log management / SIEM | `09-soc-and-ir/` |
| `Elastic Stack` | Search, analyze, visualize logs | `09-soc-and-ir/` |
| `Sigma` | Generic detection rule format | `09-soc-and-ir/` |
| `Velociraptor` | IR and endpoint hunting | `09-soc-and-ir/` |
| `Wireshark` / `tshark` | Packet capture and analysis | `03-networking/` |
| `zeek` | Network security monitoring | `09-soc-and-ir/` |

---

## Hardware / Memory Research

| Tool | Purpose | Lane |
|---|---|---|
| `rowhammer-test` | Basic DRAM bit-flip testing | `08-hardware-and-memory/` |
| `blacksmith` | Non-uniform rowhammer patterns | `08-hardware-and-memory/` |
| Custom Python scripts | Controlled memory access patterns | `08-hardware-and-memory/` |

> All rowhammer research: dedicated VM or test machine ONLY. Never on production hardware.
