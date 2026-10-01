# Common Network Security Threats — Research Report

**Author:** Mudasir Zia
**Track:** Security Analyst — OASIS INFOBYTE SIP
**Task:** Task 4 — Research Report: Common Network Security Threats

---

## 1. Introduction

Network infrastructure is the backbone of every modern organization — and the most exposed attack surface. Unlike application-layer bugs that require specific code flaws, network-level attacks exploit protocol design, trust assumptions, and traffic-handling behavior that exist by default in nearly every network. A single unpatched router, an unauthenticated ARP response, or an unmonitored DNS resolver can expose an entire organization to data theft, service outage, or full compromise. Understanding how these attacks work — not just what they're called — is foundational to any SOC or network security role, because detection and mitigation both depend on knowing the exact mechanism being abused.

---

## 2. Denial-of-Service / Distributed Denial-of-Service (DoS/DDoS)

**How it works:**
A DoS attack floods a target (server, service, or network link) with traffic or requests until it can no longer serve legitimate users. A DDoS attack does this from thousands of distributed sources simultaneously — typically a botnet of compromised IoT devices or infected hosts — making the traffic harder to filter by source IP alone. Common techniques include volumetric floods (UDP/ICMP flood), protocol attacks (SYN flood exhausting connection tables), and application-layer floods (HTTP GET/POST flood mimicking real users).

**Real-world example:**
The **2016 Dyn DNS attack** used the Mirai botnet — compromised IoT devices (cameras, routers) — to generate traffic exceeding 1 Tbps against Dyn, a major DNS provider. This took down access to Twitter, Netflix, Reddit, GitHub, and PayPal for hours across the US East Coast, demonstrating how a single infrastructure provider's outage cascades across unrelated services.

**Impact:** Revenue loss from downtime, reputational damage, SLA breaches, and — if used as a smokescreen — cover for a simultaneous data exfiltration attempt.

**Mitigations:**
1. Deploy a CDN/DDoS scrubbing service (Cloudflare, AWS Shield, Akamai) to absorb volumetric traffic before it reaches origin servers.
2. Implement rate limiting and SYN cookies at the firewall/load balancer level to blunt protocol-layer floods.
3. Maintain network redundancy (multiple DNS providers, multi-region failover) so no single point of failure takes down the whole service.

---

## 3. Man-in-the-Middle (MITM) Attacks

**How it works:**
An attacker secretly intercepts and potentially alters communication between two parties who believe they are communicating directly with each other. Common vectors include ARP spoofing on a local network (poisoning the ARP cache so traffic routes through the attacker), rogue Wi-Fi access points, SSL stripping (downgrading HTTPS to HTTP), and DNS spoofing redirecting victims to attacker-controlled servers.

**Real-world example:**
The **2015 Lenovo Superfish incident** — Lenovo pre-installed adware that injected a self-signed root CA certificate into every laptop sold. This certificate allowed the adware (and anyone who extracted the private key, which was quickly cracked) to transparently intercept and decrypt *all* HTTPS traffic on affected machines, turning every Lenovo consumer laptop into a built-in MITM vector without the user's knowledge.

**Impact:** Credential theft, session hijacking, injection of malicious content into legitimate traffic, and complete loss of confidentiality for intercepted sessions.

**Mitigations:**
1. Enforce HTTPS everywhere with HSTS (HTTP Strict Transport Security) to prevent SSL-stripping downgrades.
2. Use static ARP entries or Dynamic ARP Inspection (DAI) on managed switches to block ARP spoofing.
3. Avoid untrusted public Wi-Fi for sensitive transactions; use a VPN when unavoidable.

---

## 4. IP Spoofing

**How it works:**
An attacker forges the source IP address field in packets to impersonate a trusted host, bypass IP-based access controls, or hide the true origin of an attack. It's foundational to several other attack types — reflection/amplification DDoS (spoofing the victim's IP so replies flood them), and bypassing firewall rules that trust specific internal IP ranges.

**Real-world example:**
The **1994 Kevin Mitnick/Shimomura attack** is the textbook case — Mitnick spoofed a trusted host's IP address to exploit a TCP sequence-number prediction flaw, gaining unauthorized access to Tsutomu Shimomura's systems without ever needing valid credentials. It remains the reference case for why source-IP trust alone is an insufficient authentication mechanism.

**Impact:** Bypassed access controls, attribution evasion (attacks appear to come from innocent third parties), and amplified reflection attacks that can multiply traffic volume 50x or more.

**Mitigations:**
1. Implement **ingress/egress filtering (BCP 38)** at the network edge — routers should drop packets with source IPs that couldn't legitimately originate from that interface.
2. Use authentication mechanisms that don't rely on IP address alone (mutual TLS, signed tokens).
3. Deploy anti-spoofing features on edge routers/firewalls (Reverse Path Forwarding checks).

---

## 5. DNS Poisoning / Spoofing

**How it works:**
An attacker corrupts a DNS resolver's cache with a false IP mapping for a legitimate domain name, redirecting victims to attacker-controlled servers while the browser address bar still shows the correct domain. This can be done via cache poisoning (exploiting weak transaction-ID randomization) or by compromising the DNS server/resolver directly.

**Real-world example:**
The **2008 Kaminsky DNS vulnerability** exposed a flaw in how DNS resolvers validated responses — an attacker could predict/brute-force the 16-bit transaction ID and source port combination fast enough to inject a forged response before the real one arrived, poisoning the cache for every user of that resolver. It triggered a coordinated, multi-vendor emergency patch across virtually every DNS software vendor simultaneously.

**Impact:** Mass redirection to phishing/malware sites, credential harvesting at scale (entire ISP user bases can be affected via one poisoned resolver), and undermining the trust users place in the domain name system itself.

**Mitigations:**
1. Deploy **DNSSEC** to cryptographically sign DNS responses, preventing acceptance of forged records.
2. Use randomized source ports and transaction IDs (already standard post-Kaminsky) and keep DNS software patched.
3. Run DNS queries over encrypted channels (DNS-over-HTTPS/TLS) to prevent on-path tampering.

---

## 6. Comparison Table

| Threat | Attack Vector | Who Is at Risk | Difficulty to Execute | Ease of Mitigation |
|---|---|---|---|---|
| DoS/DDoS | Traffic/request flooding from one or many sources | Any internet-facing service | Low (botnets for hire exist) | Medium (needs CDN/scrubbing investment) |
| MITM | Interception via ARP spoof, rogue AP, SSL strip | Users on shared/untrusted networks | Medium | Medium (HSTS + DAI effective) |
| IP Spoofing | Forged source IP in packet headers | Networks trusting IP-based auth | Low–Medium | High (BCP 38 filtering is simple, underdeployed) |
| DNS Poisoning | Forged/injected DNS responses | Any DNS resolver + its users | Medium–High | Medium (DNSSEC adoption still partial) |

---

## 7. Conclusion

Three takeaways for a network administrator:

1. **Trust nothing by IP address alone** — IP spoofing and MITM both exploit networks that implicitly trust source addresses or unauthenticated local traffic. Authentication must be cryptographic, not location-based.
2. **A single compromised dependency can cascade widely** — the Dyn DDoS and Kaminsky DNS flaw both show that shared infrastructure (DNS, CDNs) multiplies the blast radius of a single attack far beyond the original target.
3. **Defense-in-depth beats any single control** — no single mitigation (a firewall, a CDN, DNSSEC) is sufficient alone; layered controls (ingress filtering + DAI + HSTS + DNSSEC + rate limiting) are what actually close these attack classes.

---

## 8. References

1. NIST — *Guide to DDoS Attacks*, NIST Special Publication 800-189 — https://csrc.nist.gov
2. CISA — *Understanding Denial-of-Service Attacks* — https://www.cisa.gov/news-events/news/understanding-denial-service-attacks
3. MITRE ATT&CK — *Network Denial of Service (T1498)* and *Adversary-in-the-Middle (T1557)* — https://attack.mitre.org
4. Kaminsky, D. (2008) — *DNS Cache Poisoning Vulnerability Disclosure* — referenced via US-CERT VU#800113
5. Krebs on Security — *Mirai Botnet / Dyn DDoS Analysis* — https://krebsonsecurity.com
