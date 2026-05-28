# 07 — Wireless & SSH

Wi-Fi attack techniques and SSH security testing.

## PDFs in This Lane

| File | Description |
|---|---|
| `Wi_Fi_hacking__.pdf` | Wi-Fi hacking techniques |
| `SSH_Access_.pdf` | SSH access and attack techniques |
| `Ssh pen testing .pdf` | SSH penetration testing |
| `ssh pentesting` | SSH pentesting notes |

## Key Tools

### Wi-Fi
- `airmon-ng` — monitor mode
- `airodump-ng` — capture handshakes
- `aireplay-ng` — deauthentication
- `aircrack-ng` — WPA/WPA2 cracking
- `hcxdumptool` + `hcxtools` — PMKID capture
- `hashcat` — GPU-accelerated cracking

### SSH
- `ssh-audit` — SSH server configuration audit
- `hydra` — SSH brute-force (authorized targets only)
- `nmap --script ssh-*` — SSH enumeration scripts
- `authorized_keys` abuse — persistence technique

## Lab Note

Wi-Fi testing requires a wireless adapter that supports monitor mode and packet injection. **Only test against access points you own or have explicit written authorization to test.**
