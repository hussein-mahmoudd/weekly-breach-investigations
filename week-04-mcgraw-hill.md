# Weekly Breach Investigation #4
## McGraw Hill — Salesforce Misconfiguration Breach
**Date:** April 11–16, 2026 | **Threat Actor:** ShinyHunters | **Type:** Cloud Misconfiguration / Data Extortion

---

## Key Stats

| Metric | Value |
|---|---|
| Accounts exposed | 13.5 million (verified by Have I Been Pwned) |
| Data leaked | 100+ GB published on dark web |
| Attack vector | Salesforce misconfiguration — no exploit required |
| Also affected | Rockstar Games, Amtrak, Hallmark Cards |

---

## 1. Executive Summary

McGraw Hill, one of the world's largest education publishers ($2.2B revenue), suffered a data breach after ShinyHunters exploited a misconfiguration in its Salesforce environment. No vulnerability or zero-day was used — attackers accessed a publicly exposed Salesforce webpage that should have required authentication. McGraw Hill refused to pay the ransom by the April 14 deadline, and ShinyHunters followed through by leaking 100+ GB of data containing 13.5 million user records.

---

## 2. Attack Timeline

| Date | Event |
|---|---|
| Early April 2026 | ShinyHunters discovers misconfigured Salesforce environment at McGraw Hill |
| Apr 11, 2026 | ShinyHunters adds McGraw Hill, Rockstar Games, Amtrak, and Hallmark to dark web leak site |
| Apr 14, 2026 | Ransom deadline passes — McGraw Hill confirms breach but refuses to pay |
| Apr 16, 2026 | ShinyHunters leaks 100+ GB of data — Have I Been Pwned verifies 13.5M affected accounts |

---

## 3. How The Attack Worked

**The misconfiguration:**
McGraw Hill deployed Salesforce Experience Cloud — a feature used to build public-facing customer portals. The configuration left internal data accessible without authentication. No hacking was required — attackers simply accessed a webpage that was unintentionally left open to the public internet.

**What was exposed:**
Names, email addresses, physical addresses, and phone numbers for 13.5 million users.

**What was NOT exposed (per McGraw Hill):**
Social Security numbers, financial data, student courseware, or internal systems.

**The extortion:**
ShinyHunters set a "pay or leak" deadline of April 14. When no payment was made, they published the full dataset on their dark web leak site.

---

## 4. MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Initial Access | Exploit Public-Facing Application (misconfigured portal) | T1190 |
| Collection | Data from Cloud Storage | T1530 |
| Exfiltration | Exfiltration Over Web Service | T1567 |
| Impact | Data Encrypted / Published for Impact | T1486 |

---

## 5. Detection Opportunities

**Log source:**
- Salesforce audit logs — access to Experience Cloud guest user sessions
- Unusual spikes in unauthenticated data access from external IPs

**Detection rule:**
- Alert on large volume data access from guest/anonymous Salesforce sessions
- Monitor for bulk record exports from Salesforce public portals

**IOCs:**
- High-volume access to Salesforce Experience Cloud pages from external IPs
- Data appearing on ShinyHunters dark web leak site
- Have I Been Pwned breach notification for company domain

---

## 6. Recommended Mitigations

1. **Audit all Salesforce Experience Cloud portals** — ensure guest user access is disabled or strictly limited on any page that could expose internal data
2. **Enable Salesforce Shield** — provides field-level encryption, event monitoring, and audit trails
3. **Test your cloud configs like an attacker would** — run regular checks on what an unauthenticated user can access from the public internet

---

## 7. Analyst Notes

**What I learned:**
No malware. No zero-day. No sophisticated attack chain. A misconfigured webpage exposed 13.5 million records. Cloud misconfigurations are now one of the most common — and most preventable — attack vectors.

**What surprised me:**
This is not McGraw Hill's first breach — they had a Salesforce-related exposure in 2022 and a PII leak in 2023. The same platform, the same type of misconfiguration, four years later.

**What I'd investigate further:**
ShinyHunters claimed 45 million records but HIBP verified 13.5 million. Where is the discrepancy? Are there more victims in the unreleased data?

---

*Investigated by: Hussein Mustafa | Part of my weekly breach investigation series*
*Previous: [Week 03 — OAuth Device Code Phishing](week-03-oauth-device-code.md) | [Week 02 — Kash Patel](week-02-kash-patel.md) | [Week 01 — Stryker](week-01-stryker.md)*
