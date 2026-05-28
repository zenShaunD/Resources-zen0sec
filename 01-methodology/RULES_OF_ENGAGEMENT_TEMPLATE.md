# Rules of Engagement (ROE) Template

**ZEN0SEC — Authorized Testing Agreement**

---

## Engagement Details

| Field | Value |
|---|---|
| **Engagement Name** | |
| **Date Range** | |
| **Operator** | Shaun Patrik Dyess / ZEN0SEC |
| **Authorizing Party** | *(System owner name / self for lab)* |
| **Authorization Type** | *(Written consent / lab self-authorization)* |

---

## Authorized Scope

### In-Scope Systems

```
# List all authorized targets:
# IP / range / hostname / URL
```

### In-Scope Techniques

- [ ] Network scanning and enumeration
- [ ] Vulnerability scanning (automated)
- [ ] Manual exploitation
- [ ] Post-exploitation / lateral movement
- [ ] Social engineering *(specify if applicable)*
- [ ] Physical testing *(specify if applicable)*
- [ ] Denial of service *(lab only, explicitly authorized)*

### Explicitly Out-of-Scope

```
# List all systems, networks, techniques that are EXCLUDED:
```

---

## Operating Constraints

- Testing window: `[START DATE/TIME]` to `[END DATE/TIME]` `[TIMEZONE]`
- Destructive techniques: *(Allowed / Not Allowed / Lab only)*
- Data exfiltration: *(Not permitted / Lab simulation only)*
- Credential usage: *(Discovered credentials only / Pre-provided)*
- Reporting deadline: `[DATE]`

---

## Emergency Stop Conditions

Testing MUST stop immediately if:

1. Any out-of-scope system is inadvertently accessed
2. Any real user data is encountered
3. Testing causes unintended service disruption outside lab
4. Authorization is verbally or in writing revoked

**Emergency contact:** `[NAME / PHONE / EMAIL]`

---

## Sign-Off

| Party | Signature | Date |
|---|---|---|
| Operator (ZEN0SEC) | | |
| Authorizing Party | | |

---

*This document must be completed and signed before any testing begins. Retain for the duration of the engagement plus 1 year.*
