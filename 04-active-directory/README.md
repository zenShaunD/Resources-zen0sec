# 04 — Active Directory

AD attack chains, post-exploitation, domain dominance techniques, and Linux-to-AD attacks.

## PDFs in This Lane

| File | Description |
|---|---|
| `AD_Attacks_.pdf` | Active Directory attack techniques |
| `AD_Post_Exploitation.pdf` | Post-exploitation in AD environments |
| `Active_Directory_Pentest_Course.pdf` | Full AD penetration testing course |
| `Attacking_Avtive_Directory_with_Linux_1762702460.pdf` | Linux-based AD attacks (Impacket, etc.) |

## Key Tools

- `BloodHound` + `SharpHound` — attack path mapping
- `CrackMapExec` — lateral movement and spraying
- `Impacket` suite — secretsdump, psexec, wmiexec, GetUserSPNs
- `Responder` — LLMNR/NBT-NS poisoning
- `kerbrute` — user enumeration and AS-REP roasting
- `mimikatz` — credential extraction (Windows target)
- `enum4linux-ng` — SMB enumeration
- `ldapdomaindump` — LDAP enumeration

## Attack Chain Reference

```
Recon → enum4linux / ldapdomaindump / BloodHound
Initial Access → AS-REP Roasting / Password Spray / LLMNR Poisoning
Lateral Movement → Pass-the-Hash / Pass-the-Ticket / SMB
Privilege Escalation → Kerberoasting / ACL abuse / DCSync
Domain Dominance → Golden Ticket / Silver Ticket / LAPS
```

## PTES Phase Mapping

- Recon: BloodHound, ldapdomaindump, enum4linux-ng
- Exploitation: Responder, kerbrute, CrackMapExec
- Post-Exploitation: mimikatz, secretsdump, DCSync
