# 09 — SOC & Incident Response

SOC operations, incident response playbooks, detection engineering, and purple team workflows.

## PDFs in This Lane

| File | Description |
|---|---|
| `SOC_IR_PLAYBOOK_DROP_1768711923.pdf` | SOC/IR operational playbook |

## Key Tools

- `Graylog` — centralized log management (recommended SIEM for this lab)
- `Elastic Stack` (ELK) — alternative SIEM
- `Sigma` — generic detection rule format (convert to any SIEM)
- `Velociraptor` — endpoint detection and IR
- `Wireshark` / `tshark` / `zeek` — network detection
- `Wazuh` — open source XDR/SIEM

## Purple Team Workflow

For every offensive exercise in any lane:
1. Run the attack
2. Check what log events were generated in SIEM
3. Write a Sigma rule to detect the technique if gap exists
4. Verify the rule fires on the next test run
5. Document in exercise report: `PLAYBOOK/EXERCISE_REPORT_TEMPLATE.md`

## MITRE ATT&CK Mapping

All findings should be tagged with MITRE ATT&CK technique IDs (e.g., T1003.001 for LSASS Memory dumping). This enables:
- Coverage tracking across techniques
- Detection gap analysis
- Purple team prioritization

Reference: https://attack.mitre.org/
