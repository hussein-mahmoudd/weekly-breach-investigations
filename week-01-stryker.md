# Weekly Breach Investigation #1
## Stryker Corporation — Handala Attack
**Date:** March 11, 2026 | **Threat Actor:** Handala (Iran-linked) | **Type:** Destructive / MDM Wipe

---

## Key Stats

| Metric | Value |
|---|---|
| Devices wiped | 200,000+ across 79 countries |
| Data exfiltrated | 50 TB |
| Attribution | Iran MOIS via Handala group |

---

## 1. Executive Summary

Stryker Corporation, a global medical device company, suffered a destructive cyberattack by Iranian-linked group Handala. Attackers compromised a Microsoft Intune admin account and issued remote wipe commands to over 200,000 devices across 79 countries, halting operations globally and disrupting medical supply chains. The U.S. DOJ officially attributed the attack to Iran's Ministry of Intelligence (MOIS) on March 20, 2026.

---

## 2. Attack Timeline

| Date | Event |
|---|---|
| Feb 28, 2026 | U.S.-Israeli strike hits a school in Tehran — cited later as the motive |
| Mar 6, 2026 | ThreatHunter.ai alerts clients of an Iranian server preparing an attack |
| Mar 11 · 3:30 AM ET | Handala issues bulk remote wipe commands via Microsoft Intune to all enrolled devices |
| Mar 11 · Morning | Employees arrive to find wiped devices displaying the Handala logo — global operations halt |
| Mar 15, 2026 | Stryker announces containment of the attack |
| Mar 20, 2026 | DOJ officially attributes attack to MOIS — FBI seizes 4 Handala domains |

---

## 3. MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Initial Access | Valid Accounts (Entra ID admin) | T1078 |
| Execution | Software Deployment Tools (Intune MDM) | T1072 |
| Impact | Data Destruction — remote wipe | T1485 |
| Impact | Disk Wipe | T1561 |
| Defense Evasion | Living off the Land — no malware used | T1562 |

---

## 4. Detection Opportunities

**Log source:**
- Microsoft Entra ID sign-in logs
- Microsoft Intune audit logs

**Detection rule:**
- Any bulk wipe command targeting more than 5 devices within one minute → trigger immediate high-severity alert
- MDM admin login from an unrecognized external IP → alert + block pending MFA re-verification

**IOCs:**
- Handala logo appearing on wiped device login screens
- MDM admin activity from unusual external IP
- Rubrik backup access outside business hours

---

## 5. Recommended Mitigations

1. **Enforce MFA on all MDM/Intune admin accounts** — a single account takeover was enough to destroy 200,000 devices
2. **Require multi-account approval for destructive Intune actions** — no single admin should be able to issue a bulk wipe unilaterally
3. **Restrict Intune admin access to named locations and trusted devices only** — no open global access

---

## 6. Analyst Notes

**What I learned:**
No malware was used — attackers weaponized a legitimate management tool. Traditional EDR would not have caught this. Identity is the new perimeter.

**What surprised me:**
Attackers also compromised Rubrik — a backup system designed to be immutable — before executing the wipe, ensuring no recovery was possible.

**What I'd investigate further:**
How exactly were the Entra ID admin credentials obtained? Phishing? Supply chain compromise? This initial access vector has not been officially confirmed yet.

---

*Investigated by: [hussein] | Part of my weekly breach investigation series*
