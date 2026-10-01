# 🩹 The Importance of Patch Management — Research Report

**Author:** Mudasir Zia · **Track:** Security Analyst — OASIS INFOBYTE SIP · **Task:** 6

---

> 💡 **Core truth:** Most breaches don't exploit secret, unknown flaws. They exploit **known** vulnerabilities that already had a fix available — sometimes for months or years. Patch management isn't a chore; it's the single highest-ROI control in security.

---

## 📊 Quick Stats

| Stat | Why It Matters |
|---|---|
| **60%** of breach victims said they were breached due to an unpatched known vulnerability | Ponemon Institute |
| **300,000+** machines infected in **4 days** by WannaCry (2017) | Unpatched SMB flaw, patch available 2 months prior |
| **147 million** people affected by Equifax breach (2017) | Apache Struts flaw unpatched for 2+ months after disclosure |

---

## 🔍 1. What Is Patch Management?

Patch management is the structured process of identifying, acquiring, testing, and deploying code updates that fix security vulnerabilities, bugs, or performance issues in software and systems. It is the operational core of the **vulnerability lifecycle**: a flaw is discovered → cataloged as a **CVE** (Common Vulnerabilities and Exposures) → scored for severity (**CVSS**) → a vendor releases a patch → organizations must deploy it before attackers weaponize it. The gap between "patch available" and "patch applied" is the exact window attackers live in.

---

## 🚨 2. Why Patches Matter — Real Breaches Caused by Not Patching

<details>
<summary>📁 WannaCry / EternalBlue (2017) — click to expand</summary>

Microsoft released a patch (MS17-010) for the **EternalBlue** SMB vulnerability in **March 2017**. The **WannaCry** ransomware worm, launched in **May 2017**, exploited that exact same unpatched flaw — infecting over 300,000 machines across 150 countries in days, including the UK's NHS, shutting down hospitals and forcing ambulance diversions. The patch had existed for **two months** before the attack.

</details>

<details>
<summary>📁 Equifax Breach (2017) — click to expand</summary>

A critical vulnerability in **Apache Struts** (CVE-2017-5638) was publicly disclosed with a patch in **March 2017**. Equifax failed to apply it. Attackers exploited the exact flaw starting **May 2017**, exfiltrating sensitive data (SSNs, birth dates, addresses) for **147 million people** over several months before detection. The breach cost Equifax an estimated **$1.4 billion**.

</details>

---

## ⚠️ 3. Consequences of Not Patching

| Consequence | Impact |
|---|---|
| 💸 **Financial** | Breach costs, fines, remediation — often in the millions |
| 🔒 **Ransomware** | Unpatched systems are the #1 entry point for ransomware deployment |
| ⚖️ **Compliance violations** | GDPR, HIPAA, PCI-DSS all mandate timely patching — failure triggers penalties |
| 📉 **Reputational damage** | Customer trust loss, stock price drops (Equifax stock fell ~35% post-disclosure) |

---

## 🔄 4. The Patch Management Lifecycle

```
Discovery → Assessment → Testing → Deployment → Verification
```

| Phase | What Happens |
|---|---|
| **1. Discovery** | Scan environment to identify systems missing patches (vulnerability scanners: Nessus, Qualys) |
| **2. Assessment** | Prioritize by CVSS severity + asset criticality — not every patch is equally urgent |
| **3. Testing** | Apply patch in a staging/test environment to catch compatibility breaks before production |
| **4. Deployment** | Roll out to production — staged/phased rollout to limit blast radius if something breaks |
| **5. Verification** | Confirm patch applied successfully and vulnerability is actually closed (re-scan) |

---

## ✅ 5. Best Practices — 7-Step Checklist

- [ ] **1.** Maintain a full, current asset inventory — you can't patch what you don't know exists
- [ ] **2.** Run automated vulnerability scans on a recurring schedule (weekly minimum)
- [ ] **3.** Prioritize patches by CVSS score **and** exploitability-in-the-wild (check CISA's KEV catalog)
- [ ] **4.** Always test patches in staging before production deployment
- [ ] **5.** Set a defined SLA for critical patches (e.g., 72 hours for critical, 30 days for medium)
- [ ] **6.** Automate deployment where possible (WSUS, SCCM, Ansible) to reduce human delay
- [ ] **7.** Verify and document — re-scan post-deployment and keep an audit trail for compliance

---

## 🧱 6. Challenges & How to Overcome Them

| Challenge | Why It Happens | Fix |
|---|---|---|
| **Legacy systems** | Old software may break with patches, or vendor no longer supports it | Network segmentation/isolation of legacy systems that can't be patched |
| **Downtime concerns** | Patching critical systems means scheduled outages | Use phased/rolling deployments; patch in maintenance windows |
| **Testing overhead** | Thorough testing takes time orgs don't feel they have | Automate regression testing; risk-accept low-impact patches faster |
| **Patch volume fatigue** | Hundreds of CVEs monthly overwhelm small teams | Risk-based prioritization (CVSS + KEV) instead of patching everything equally |

---

## 📚 References

1. NIST — *Special Publication 800-40 Rev. 4: Guide to Enterprise Patch Management Planning* — https://nvlpubs.nist.gov
2. CISA — *Known Exploited Vulnerabilities (KEV) Catalog* — https://www.cisa.gov/known-exploited-vulnerabilities-catalog
3. MITRE — *CVE Database* — https://cve.mitre.org
4. FIRST.org — *CVSS Scoring System* — https://www.first.org/cvss
5. Krebs on Security — *Equifax Breach Timeline & Analysis* — https://krebsonsecurity.com
