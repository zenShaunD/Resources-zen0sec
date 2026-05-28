# 08 — Hardware & Memory Research

Rowhammer DRAM vulnerability research — academic and practical.

## PDFs in This Lane

| File | Description |
|---|---|
| `Gpu rowhammer.pdf` | GPU-based Rowhammer attack research |
| `TRR rowhammer.pdf` | TRR (Target Row Refresh) bypass research |
| `blacksmith_sp22_Rowhammer.pdf` | Blacksmith: Non-uniform rowhammer patterns (2022) |
| `Rowhammer survey.pdf` | Comprehensive Rowhammer survey |
| `Solving rowhammer.pdf` | Rowhammer mitigations and solutions |

## What is Rowhammer?

Rowhammer is a DRAM vulnerability where repeatedly accessing ("hammering") rows of memory causes bit flips in adjacent rows. This can be exploited to:
- Escalate privileges (flip bits in page tables)
- Break out of sandboxes
- Corrupt cryptographic keys

## Research Scope

All Rowhammer research in this lab is:
- Academic and defensive in nature
- Conducted on dedicated test hardware or VMs only
- Never targeted at production systems or third-party hardware

## Key Tools

- `rowhammer-test` (Google Project Zero) — basic bit-flip testing
- `blacksmith` — non-uniform hammering patterns
- Custom Python scripts for controlled memory access patterns
- `perf` — hardware performance counters for cache flush analysis

## Research Path

1. Read: `Rowhammer survey.pdf` (foundational overview)
2. Read: `blacksmith_sp22_Rowhammer.pdf` (state-of-art techniques)
3. Read: `TRR rowhammer.pdf` (mitigation bypass)
4. Read: `Gpu rowhammer.pdf` (GPU attack surface)
5. Read: `Solving rowhammer.pdf` (defensive perspective)
