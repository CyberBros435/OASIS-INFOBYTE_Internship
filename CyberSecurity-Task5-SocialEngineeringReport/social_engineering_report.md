# 🧠 Social Engineering Attacks — Research Report

**Author:** Mudasir Zia · **Track:** Security Analyst — OASIS INFOBYTE SIP · **Task:** 5

---

> 💡 **The core truth of this report:** Every firewall, EDR, and SIEM in the world fails against one attack vector — a human who trusts the wrong message. Social engineering doesn't break systems. It breaks *judgment*.

---

## 📊 Why This Matters — At a Glance

| Stat | Source |
|---|---|
| **~90%** of successful breaches start with a phishing email | Verizon DBIR |
| **$4.9M** average cost of a breach involving phishing | IBM Cost of a Data Breach Report |
| **1 in 4** employees click a simulated phishing link in training tests | Industry benchmark average |

> ⚠️ Social engineering is considered the #1 attack vector precisely *because* it skips technical defenses entirely — it attacks the one component no patch can fix: human decision-making under pressure.

---

## 🎣 1. Phishing

| | |
|---|---|
| **Types** | Spear Phishing (targeted individual), Whaling (targets executives), Vishing (voice/phone), Smishing (SMS) |
| **Risk Level** | 🔴 Critical |

**How it works:** Attacker sends a message impersonating a trusted source (bank, coworker, IT dept) to trick the victim into clicking a malicious link, entering credentials, or transferring funds.

<details>
<summary>📁 Case Study — click to expand</summary>

**2020 Twitter Bitcoin Scam Hack:** Attackers used **vishing** — phone calls impersonating Twitter IT support — to trick employees into handing over internal admin tool credentials. This gave attackers control of 130 high-profile accounts (Obama, Musk, Apple), used to run a Bitcoin scam that netted ~$120,000 in under an hour. No malware, no exploit — just a convincing phone call.

</details>

**✅ Prevention:**
1. Mandatory phishing-simulation training with measurable click-rate tracking
2. Email authentication: SPF, DKIM, DMARC enforced on all domains
3. Multi-factor authentication (MFA) so stolen credentials alone aren't enough
4. "Verify out-of-band" policy — any urgent credential/financial request gets a callback on a known number, never the number in the message

---

## 🎭 2. Pretexting

| | |
|---|---|
| **Definition** | Attacker fabricates a believable scenario ("pretext") to extract information or access |
| **Risk Level** | 🟡 High |

**How it works:** The attacker builds a false identity/scenario — IT support, auditor, new hire, vendor — and uses that false authority to request sensitive info the victim would normally protect.

<details>
<summary>📁 Case Study — click to expand</summary>

**2011 RSA SecurID Breach:** Attackers sent a targeted email titled "2011 Recruitment Plan" to a small group of RSA employees. One opened the attached Excel file containing a zero-day Flash exploit — but the pretext (a believable HR document) is what got it opened at all. The resulting breach compromised the seed data behind SecurID tokens used by thousands of client organizations worldwide, including defense contractors.

</details>

**✅ Prevention:**
1. Strict identity-verification protocol before releasing any sensitive data, regardless of claimed authority
2. "Need to know" access control — minimize how much any single role can access/disclose
3. Security awareness training specifically on authority-based manipulation tactics

---

## 🎁 3. Baiting

| | |
|---|---|
| **Types** | Physical (infected USB left in parking lot), Digital (fake "free download") |
| **Risk Level** | 🟡 High |

**How it works:** Attacker exploits curiosity or greed by leaving a tempting "bait" — a labeled USB drive, a fake free-software download — that deploys malware the moment it's used.

<details>
<summary>📁 Case Study — click to expand</summary>

**Stuxnet (2010):** While Stuxnet is famous as a nation-state cyberweapon, its initial delivery into Iran's air-gapped Natanz facility relied on **baiting** — infected USB drives introduced into the facility by unwitting insiders or contractors. No network connection was needed; human curiosity (plugging in a found/handed drive) was the entire delivery mechanism.

</details>

**✅ Prevention:**
1. Disable autorun/autoplay on all removable media org-wide
2. Endpoint policy blocking unauthorized USB devices (allowlist only)
3. Train staff: never plug in unknown media — report it to security instead

---

## 🔄 4. Quid Pro Quo *(Bonus)*

**How it works:** Attacker offers a service or benefit ("free IT support," "prize") in exchange for information or access — exploiting reciprocity instinct rather than fear or authority.

**Prevention:** Treat all unsolicited "help" offers as suspicious by default; route all real IT support requests through a verified internal ticketing system only.

---

## 📋 Comparison Table

| Attack Type | Primary Target | Psychological Lever | Best Countermeasure |
|---|---|---|---|
| Phishing | Any employee with email/phone | Urgency, fear, authority | MFA + email authentication (DMARC) |
| Pretexting | Specific roles (HR, finance, IT) | Trust in authority/identity | Strict verification protocol |
| Baiting | Curious/opportunistic individuals | Curiosity, greed | Endpoint USB lockdown |
| Quid Pro Quo | Anyone wanting help/reward | Reciprocity | Verified-channel-only support requests |

---

## ✅ Employee Security Awareness Checklist

- [ ] 1. Run quarterly phishing simulations with tracked click/report rates
- [ ] 2. Enforce MFA on every account with access to sensitive systems
- [ ] 3. Train staff to verify unusual requests out-of-band (call back, don't reply)
- [ ] 4. Lock down USB/removable media at the endpoint level
- [ ] 5. Establish a clear, blame-free reporting channel for "I think I clicked something"

---

## 📚 References

1. Verizon — *2024 Data Breach Investigations Report (DBIR)* — https://www.verizon.com/business/resources/reports/dbir/
2. IBM — *Cost of a Data Breach Report* — https://www.ibm.com/reports/data-breach
3. CISA — *Social Engineering and Phishing Guidance* — https://www.cisa.gov
4. Krebs on Security — *Twitter 2020 Hack Breakdown* — https://krebsonsecurity.com
5. RSA / Krebs on Security — *Anatomy of the RSA SecurID Breach* — https://krebsonsecurity.com
