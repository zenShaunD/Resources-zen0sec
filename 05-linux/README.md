# 05 — Linux

Linux penetration testing, privilege escalation techniques, and living-off-the-land.

## PDFs in This Lane

| File | Description |
|---|---|
| `Linux_Pentest__1738352163.pdf` | Linux penetration testing full course |
| `Linux_Privilege_Escalation.pdf` | Linux privilege escalation techniques |
| `LoLbas1pg.pdf` | Living Off the Land Binaries and Scripts (LoLBAS) one-pager |

## Key Tools

- `LinPEAS` — automated privesc enumeration
- `pspy` — process monitoring without root
- `GTFOBins` — GTFO binaries for privilege escalation
- `sudo -l` — sudo misconfiguration check
- `find / -perm -4000` — SUID binary discovery
- `linuxprivchecker.py` — Python-based privesc checker

## Privesc Checklist

```
[ ] sudo -l (misconfigured sudo)
[ ] SUID binaries (find / -perm -4000 2>/dev/null)
[ ] World-writable scripts in cron
[ ] Writable /etc/passwd or /etc/shadow
[ ] Path hijacking opportunities
[ ] NFS shares with no_root_squash
[ ] Kernel version exploits (uname -a)
[ ] Capabilities (getcap -r / 2>/dev/null)
[ ] Docker socket access
```
