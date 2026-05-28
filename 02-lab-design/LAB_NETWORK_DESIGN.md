# ZEN0SEC Lab Network Design

**Operator:** Shaun Patrik Dyess / ZEN0SEC  
**Platform:** Workstation Tower — Ubuntu 24.04 LTS + Parrot OS 7.2 Security (dual-boot)  
**Purpose:** Isolated offensive security research lab

---

## Design Principles

1. **Complete isolation** — zero route between lab network and home LAN or public internet
2. **Segmented attack surface** — separate subnets per function
3. **Full observability** — all traffic captured and logged for both offense and defense learning
4. **Snapshot-first discipline** — every target VM snapshotted before any exercise
5. **Purple team ready** — attacker and defender tooling both present

---

## Network Topology

```
┌─────────────────────────────────────────────────────────┐
│              WORKSTATION TOWER                        │
│         Ubuntu 24.04 / Parrot OS 7.2                  │
│                                                       │
│  ┌───────────────┐   ┌───────────────┐             │
│  │  HYPERVISOR    │   │  VM MANAGER    │             │
│  │  VirtualBox or │   │  (virt-manager/│             │
│  │  VMware WS Pro │   │  libvirt/KVM)  │             │
│  └───────────────┘   └───────────────┘             │
│                                                       │
│  VIRTUAL NETWORKS (host-only / internal, NO NAT):     │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │ SEGMENT A — Attacker          10.10.10.0/24    │  │
│  │   Parrot OS 7.2 attacker VM   10.10.10.10       │  │
│  └──────────────────────────────────────────────┘  │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │ SEGMENT B — Active Directory  10.10.20.0/24    │  │
│  │   Windows Server 2019/2022 DC 10.10.20.10       │  │
│  │   Windows 10/11 workstation   10.10.20.20       │  │
│  │   (optional) Linux domain box 10.10.20.30       │  │
│  └──────────────────────────────────────────────┘  │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │ SEGMENT C — DMZ / Web Targets 10.10.30.0/24    │  │
│  │   Vulnerable web app VMs       10.10.30.10-50   │  │
│  │   (DVWA, VulnHub, HackTheBox   │  │
│  │    offline OVAs, Metasploitable)│  │
│  └──────────────────────────────────────────────┘  │
│                                                       │
│  ┌──────────────────────────────────────────────┐  │
│  │ SEGMENT D — Management/Logging 10.10.40.0/24   │  │
│  │   Ubuntu log host / SIEM        10.10.40.10      │  │
│  │   (Graylog or ELK stack)                         │  │
│  │   Wireshark/tshark capture pts  all segments     │  │
│  └──────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘

    NO ROUTE TO HOME LAN (192.168.x.x) OR INTERNET
    All segments: host-only / internal network only
```

---

## Segment Details

### Segment A — Attacker (10.10.10.0/24)

| VM | OS | IP | Role |
|---|---|---|---|
| zen0-attacker | Parrot OS 7.2 Security | 10.10.10.10 | Primary attack platform |

**Tooling present:** Full Parrot Security suite, ZEN0SEC scripts lib, Python orchestrators

### Segment B — Active Directory (10.10.20.0/24)

| VM | OS | IP | Role |
|---|---|---|---|
| zen0-dc01 | Windows Server 2019/2022 | 10.10.20.10 | Domain Controller |
| zen0-ws01 | Windows 10/11 | 10.10.20.20 | Domain Workstation |
| zen0-linux-member | Ubuntu 22.04 | 10.10.20.30 | Linux domain-joined box |

**Use:** AD attack/defense exercises, BloodHound mapping, Kerberoasting, DCSync, lateral movement

### Segment C — DMZ / Web Targets (10.10.30.0/24)

| VM | OS | IP | Role |
|---|---|---|---|
| dvwa | Ubuntu + DVWA | 10.10.30.10 | Deliberately Vulnerable Web App |
| metasploitable | Metasploitable 2/3 | 10.10.30.20 | Multi-service vulnerable target |
| custom-web | Ubuntu + LAMP | 10.10.30.30 | Custom web app testing |

**Use:** OWASP WSTG exercises, SQLi, IDOR, JWT, WAF bypass, Burp Suite workflows

### Segment D — Management / Logging (10.10.40.0/24)

| VM | OS | IP | Role |
|---|---|---|---|
| zen0-siem | Ubuntu 22.04 | 10.10.40.10 | Central log host (Graylog/ELK) |

**Use:** Receive syslog/winlog from all segments, Wireshark captures, purple team detection engineering

---

## Isolation Enforcement

```bash
# VirtualBox: set all lab adapters to "Host-Only" or "Internal Network"
# NEVER use NAT or Bridged for lab VMs

# Verify no default route to host LAN exists inside lab VM:
ip route show
# Should show ONLY 10.10.x.x routes, no 0.0.0.0/0 or 192.168.x.x

# Verify no internet connectivity from lab VM:
curl -m 5 https://example.com  # should timeout/fail
```

---

## Logging Architecture

- **Attacker VM:** `script` command for terminal logging, or `tmux` + logging plugin
- **Windows VMs:** WinRM/WEF forwarding to SIEM
- **Linux VMs:** `rsyslog` forwarding to 10.10.40.10
- **Network:** Wireshark/tshark span port capture on hypervisor vSwitch
- **SIEM:** Graylog (recommended) or ELK stack on 10.10.40.10

---

## Pre-Exercise Checklist

```
[ ] All target VMs snapshotted
[ ] Lab network confirmed isolated (no internet route)
[ ] Logging/capture running on Segment D
[ ] ROE / exercise scenario document written
[ ] Attacker VM terminal logging enabled
[ ] Objectives defined with PTES phase tags
```

---

## Post-Exercise Cleanup

```
[ ] Remove all persistence mechanisms created during exercise
[ ] Delete created user accounts on target VMs
[ ] Restore VMs to snapshot if needed
[ ] Review logs and write exercise report
[ ] Note detection gaps for purple team follow-up
```
