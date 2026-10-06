# 🔍 Task 1 — Basic Network Scanning with Nmap

**Author:** Mudasir Zia · **Track:** Security Analyst — OASIS INFOBYTE SIP · **Task:** 1

---

## 🧰 What Is Nmap?

Nmap (Network Mapper) is an open-source tool for network discovery and security auditing. It sends crafted packets to a target and analyzes the responses to determine: which hosts are alive, which ports are open, what services are running on those ports, and (optionally) the target's operating system. It's the foundational recon tool in both offensive pentesting and defensive SOC work — you can't secure what you don't know is exposed.

> ⚠️ **Ethics Note:** Only scan systems you own or have explicit written permission to scan. This exercise was run entirely against `127.0.0.1` (localhost) inside a local Kali VM — no external or third-party systems were touched.

---

## ⚙️ Setup

Nmap ships pre-installed on Kali Linux — no install step needed.

```bash
nmap --version   # confirm install
```

---

## 🧪 Scans Performed

| Scan | Command | Purpose |
|---|---|---|
| Basic | `nmap 127.0.0.1` | Discover open ports |
| Service Version | `nmap -sV 127.0.0.1` | Identify software + version behind each port |
| OS Detection | `nmap -O 127.0.0.1` | Fingerprint the target OS via TCP/IP stack behavior |

**Initial result:** All three scans against a clean `127.0.0.1` returned **0 open ports** — expected, since a default Kali install runs no listening services. To produce analyzable findings (as the task requires), a lightweight HTTP service was started intentionally:

```bash
python3 -m http.server 8080 &
nmap -sV -O 127.0.0.1 > nmap_scan_results.txt
```

---

## 📊 Findings

| Port | State | Service | Version |
|---|---|---|---|
| **8080/tcp** | ✅ Open | HTTP | SimpleHTTPServer 0.6 (Python 3.13.12) |
| **999 other ports** | ❌ Closed | — | — |

**OS Detection result:** No exact OS match — Kali's networking stack didn't match Nmap's fingerprint database closely enough for a confident guess (common on VMs/virtualized NICs where timing characteristics differ from bare metal).

---

## 🔬 Port Analysis — Is This a Security Risk?

### Port 8080 — Python SimpleHTTPServer

**What it does:** Serves files from the current directory over plain HTTP — Python's built-in dev server, meant for quick local file sharing/testing, never for production.

**Security risk: 🔴 Yes, if exposed beyond localhost.**
- **No authentication** — anyone who can reach the port can browse and download every file in the served directory
- **No encryption** — all traffic is plaintext HTTP, trivially sniffable on a shared network
- **No access logging/hardening** — it logs requests to console only, no persistent audit trail, no rate limiting

Because this instance was bound to `127.0.0.1` only (not `0.0.0.0` on a routable interface), it was **not actually internet-exposed** during this test — but the exact same command run with network exposure (common dev mistake) would expose the entire directory to anyone on the network/internet.

**Fix if this were real:** Never run `SimpleHTTPServer` outside isolated local testing. Use a properly authenticated, TLS-terminated web server (nginx + auth) for anything beyond a throwaway local test.

---

## 🧩 Interesting Detail: Nmap's Own Probe Traffic

The HTTP server's access log captured requests like `GET /HNAP1`, `GET /evox/about`, `POST /sdk` — these are **not external attackers**. They're Nmap's `-sV` service-detection engine deliberately sending known device/malware-signature request paths (originally associated with IoT botnet exploit patterns like Mirai) to see how the target responds, which helps it fingerprint the exact software/version running. Recognizing this distinction — "is this traffic from my own scan, or something else" — is a core skill for SOC log triage.

---

## ✅ Summary

- **Triage:** 🟢 Low (test environment, localhost-bound, no real exposure)
- **Evidence:** Port 8080/tcp open — SimpleHTTPServer 0.6 / Python 3.13.12
- **Action:** Document as a controlled demo; in production, this service class (unauthenticated HTTP file server) should never be reachable off localhost
- **Escalate:** NO — isolated lab exercise, no real-world exposure
