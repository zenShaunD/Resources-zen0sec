# 11 — Tools & References

Tool references, cheat sheets, and specialized attack technique guides.

## PDFs in This Lane

| File | Description |
|---|---|
| `LoLbas1pg.pdf` | Living Off the Land Binaries and Scripts reference |
| `Docker pen test.pdf` | Docker and container penetration testing |
| `DDoS_Attack__1738356069.pdf` | DDoS attack techniques (research/lab use) |
| `curl_cheatsheet.pdf` | curl command reference |
| `Websites` | Web resource links |

## LoLBAS Quick Reference

Living Off the Land = using legitimate system binaries for offensive purposes.
- Windows: `certutil`, `mshta`, `regsvr32`, `rundll32`, `wmic`, `powershell`
- Linux: `bash`, `python`, `perl`, `awk`, `find`, `curl`, `wget`
- Full reference: https://lolbas-project.github.io/ (Windows), https://gtfobins.github.io/ (Linux)

## Docker Security Testing

Key attack surfaces:
- Exposed Docker socket (`/var/run/docker.sock`)
- Privileged containers
- Container escape techniques
- Image vulnerability scanning with `trivy`
