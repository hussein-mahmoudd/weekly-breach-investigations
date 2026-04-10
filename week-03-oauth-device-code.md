# Weekly Breach Investigation #3
## EvilTokens — OAuth Device Code Phishing Campaign
**Date:** February 19 – April 2026 (ongoing) | **Threat Actor:** EvilTokens PhaaS | **Type:** MFA Bypass / Token Hijacking

---

## Key Stats

| Metric | Value |
|---|---|
| Organizations hit | 340+ across 7 countries |
| Countries targeted | US, Canada, Australia, New Zealand, Germany, France, UAE |
| Sectors affected | Healthcare, finance, legal, government, manufacturing |
| Attack type | OAuth device code phishing — MFA bypass |

---

## 1. Executive Summary

A large-scale phishing campaign exploited Microsoft's legitimate OAuth device authorization flow to bypass MFA and steal persistent access tokens from 340+ Microsoft 365 organizations. The attackers used a Phishing-as-a-Service platform called EvilTokens, sold on Telegram since February 2026. The victim authenticates on the real Microsoft login page and completes MFA themselves — unknowingly handing the attacker a valid token that persists even after a password reset.

---

## 2. Attack Timeline

| Date | Event |
|---|---|
| Feb 19, 2026 | Campaign first detected by Huntress — initial cases observed |
| Feb–Mar 2026 | EvilTokens PhaaS platform goes on sale via Telegram, accelerating attack volume |
| Mar 25, 2026 | Cloud Security Alliance publishes research — 340+ orgs confirmed compromised |
| Mar 30, 2026 | Sekoia publishes full EvilTokens analysis — campaign still active and expanding |
| Apr 2026 | Campaign ongoing — attacker plans to add Gmail and Okta phishing support |

---

## 3. Attack Flow (Step by Step)

1. Victim receives a phishing email impersonating DocuSign, Adobe Acrobat, or SharePoint
2. Email contains a verification code and a "Continue to Microsoft" button
3. Button redirects to the **real** Microsoft device login page
4. Victim enters the code and completes MFA on genuine Microsoft infrastructure
5. Attacker receives valid access token + long-lived refresh token
6. Attacker now has full Microsoft 365 access — email, files, Teams — even after a password reset

---

## 4. MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Initial Access | Phishing — Spearphishing Link | T1566.002 |
| Credential Access | Steal Application Access Token | T1528 |
| Defense Evasion | Use Alternate Authentication Material | T1550.001 |
| Persistence | Valid Accounts — Cloud Accounts | T1078.004 |
| Collection | Email Collection | T1114 |

---

## 5. Detection Opportunities

**Log source:**
- Microsoft Entra ID / Azure AD sign-in logs
- Microsoft 365 audit logs

**Detection rules:**
- Alert on unusual spikes in Device Code authentication flow across the tenant
- Alert on successful device code auth from unrecognized IP or country
- Hunt for new mail forwarding rules or OAuth apps registered after a device code auth event

**IOCs:**
- Authentication events from Railway.com infrastructure (162.220.234[.]41)
- Cloudflare Workers redirect URLs in phishing emails
- Device code flow usage in organizations that do not use IoT or limited-input devices

---

## 6. Recommended Mitigations

1. **Disable device code authentication flow in Azure AD Conditional Access** if your organization does not use IoT or limited-input devices — this eliminates the attack vector entirely
2. **Revoke all refresh tokens** for any affected users immediately (Revoke-AzureADUserAllRefreshToken)
3. **Train employees** to never enter a verification code they did not personally initiate — legitimate services do not email you a code and ask you to enter it on a separate page

---

## 7. Analyst Notes

**What I learned:**
MFA is not a silver bullet. Device code phishing bypasses MFA completely because the victim completes the challenge themselves on the real Microsoft site. The attack abuses a legitimate protocol — not a vulnerability. This is why token-based attacks are becoming the new standard.

**What surprised me:**
EvilTokens is sold on Telegram as a turnkey service — no technical skills required. Anyone can buy it and launch this attack. This is the full commoditization of sophisticated MFA bypass techniques.

**What I'd investigate further:**
How many of the 340+ compromised organizations detected the breach before data was exfiltrated? The refresh token persistence is the most dangerous part — how long did attackers maintain access before detection?

---

*Investigated by: Hussein Mustafa | Part of my weekly breach investigation series*
*Previous: [Week 02 — Kash Patel Gmail Breach](week-02-kash-patel.md) | [Week 01 — Stryker Corporation](week-01-stryker.md)*
