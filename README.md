# 🛡️ Networkwalks B082 — Week 2: Footprinting, Reconnaissance & Network Scanning

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-red) ![Skill](https://img.shields.io/badge/Skill-Ethical%20Hacking-red) ![Tool](https://img.shields.io/badge/Tool-Kali%20Linux-informational) ![Tool](https://img.shields.io/badge/Tool-Maltego-informational) ![Tool](https://img.shields.io/badge/Tool-theHarvester-informational) ![Tool](https://img.shields.io/badge/Tool-Nmap%2FZenmap-informational) ![Program](https://img.shields.io/badge/Networkwalks-B082-blue)

## 📌 Project Overview

This repository documents **Week 2** of my Cybersecurity & Ethical Hacking internship at **Networkwalks** (Batch B082). Week 2 covers **Phase 1 (Reconnaissance & Footprinting)** and **Phase 2 (Scanning & Network Discovery)** of the penetration-testing lifecycle.

Per the program's Week 2 requirements (minimum: 1 elective module + both essential modules), I completed **all four elective footprinting modules** plus both essential modules:

| Module | Description | Status |
|---|---|---|
| W2-PM1 | Footprinting with multiple Kali Linux tools (whois, whatweb, nslookup, curl, wafw00f, dnsrecon) | ✅ Complete |
| W2-PM2 | Footprinting with the Google Hacking Database (GHDB) | ✅ Complete |
| W2-PM3 | Footprinting with Maltego | ✅ Complete |
| W2-PM4 | Footprinting with theHarvester | ✅ Complete |
| W2-PM5 | Network scanning with Zenmap *(essential)* | ✅ Complete |
| W2-PM-FINAL | Final consolidated report *(essential)* | ✅ Complete |

## 🎯 Objectives

- Build a public information profile of a live domain (`networkwalks.com`) using six command-line reconnaissance tools.
- Use Google dorking / GHDB to locate publicly indexed but sensitive listings.
- Use Maltego to graphically map a domain to related emails and URLs.
- Use theHarvester to harvest emails and sub-domains for a target domain from public sources.
- Discover live hosts, IP/MAC addresses and topology on my own local network using Zenmap.
- Document every finding professionally, including its risk level and a recommended mitigation.

## 🧰 Tools Used

`Kali Linux` · `WHOIS` · `WhatWeb` · `Nslookup` · `Curl` · `Wafw00f` · `DNSrecon` · `Google Hacking Database (GHDB)` · `Maltego Desktop CE` · `theHarvester` · `Zenmap (Nmap GUI)` · `Windows CMD (ipconfig)`

## 🔍 Methodology

Each module followed the same four-step approach: **Prepare → Execute → Capture → Analyse** — confirm scope, run the documented command/action, save the output and a screenshot as evidence, then analyse the finding from an attacker's perspective.

## 📜 Scope & Authorization

All testing against `networkwalks.com` was carried out under a signed Letter of Authorization (Ref. NW-LOA-B082-017) issued by Networkwalks for the internship period (17–24 Aug 2026). My own LAN was tested under owner's consent. `microsoft.com` was queried only through theHarvester's passive, public-source lookups — no direct connection was made to that domain's infrastructure. Exploitation, unauthorized access, brute-forcing and social engineering were explicitly out of scope.

---

## 🧪 Module Write-Ups

### 6.1 — Footprinting with Multiple Kali Linux Tools (`networkwalks.com`)

| Tool | Command | Key Finding |
|---|---|---|
| **WHOIS** | `whois networkwalks.com` | Registered via GoDaddy, hosted on HostGator name servers, registrant privacy-protected (Domains By Proxy), DNSSEC unsigned |
| **WhatWeb** | `whatweb networkwalks.com` | WordPress 7.0.4 + WP Download Manager 3.3.58 on Apache; server IP and a contact e‑mail exposed |
| **Nslookup** | `nslookup networkwalks.com` | Domain resolves to a public IPv4 address *(redacted here — see private report)* |
| **Curl -I** | `curl -I networkwalks.com` | Headers revealed the active security plugin ("Really Simple Security") and a caching/CDN layer |
| **Wafw00f** | `wafw00f networkwalks.com` | Site protected by **ModSecurity (SpiderLabs)** WAF |
| **DNSrecon** | `dnsrecon -d networkwalks.com` | 8 DNS records enumerated (NS, MX, TXT/SPF, SRV) plus the DNS server software version |

**Attacker's perspective:** version numbers, exposed headers and DNS metadata let an attacker fingerprint the whole stack and cross-reference public CVE databases — without ever logging in or touching the target directly.

### 6.2 — Footprinting with the Google Hacking Database (GHDB)

**Task 1 — Exposed / live security cameras.** Using classic GHDB dorks (`intitle:"webcamXP" inurl:8080`, `inurl:/multi.html intitle:webcam`, etc.) I located **10 internet-reachable camera/IoT interfaces**. 🔒 *Exact host:port values and direct links are withheld from this public repo per the program's guidance on not publishing directly reachable device links — see the private submitted report.*

**Task 2 — Publicly indexed mathematics PDF eBooks.** Using `intitle:index.of "parent directory" mathematics pdf`, I located **10 open directory listings** exposing downloadable PDFs — all academic/university mirrors, e.g. `skylineuniversity.ac.ae/pdf/math/`, `math.dartmouth.edu/~carlp/PDF/`, `math.ucla.edu/~popa/Books/`, `netlib.org/math/docpdf/` (full list of 10 in the submitted report).

**Attacker's perspective:** the finding itself — a device or directory being publicly indexed at all — is the exposure. Directory indexing left enabled on a different target could just as easily reveal configs, backups or credentials instead of harmless PDFs.

### 6.3 — Footprinting with Maltego

A Domain entity for `networkwalks.com` was expanded with the built-in **"To Email Addresses [Utilities]"** transform, returning **1 e‑mail address** and **2 related URLs**, visualised as a link-analysis graph.

**Attacker's perspective:** Maltego's value is the *graph*, not the single data point — it pivots automatically from a domain to emails, URLs and (with further transforms) infrastructure, mapping an org's public attack surface fast.

### 6.4 — Footprinting with theHarvester (`microsoft.com`, passive OSINT only)

| Run | Source | Result |
|---|---|---|
| 1 | `theHarvester -d microsoft.com -l 1000 -b baidu` (1st) | 0 emails, 4 hosts |
| 2 | `theHarvester -d microsoft.com -l 1000 -b baidu` (2nd) | 3 emails, 6 hosts |
| 3 | `theHarvester -d microsoft.com -l 50 -b all` | Most premium sources returned "Missing API key" on a fresh Kali install — free/keyless sources still worked |

**Attacker's perspective:** every harvested e‑mail is a potential phishing target, and every sub-domain is a possible forgotten entry point — all gathered without a single packet reaching Microsoft's own servers.

### 6.5 — Network Scanning with Zenmap (own LAN)

`nmap -sn <own /24 subnet>` (Zenmap "Ping scan" profile) discovered **25 live hosts** on my local subnet, including router/gateway devices (MikroTik), Intel-based PCs and AzureWave Wi‑Fi/IoT modules. A full topology diagram was exported. 🔒 *Exact IP/MAC address table withheld here — see private report.*

**Attacker's perspective:** a ping scan alone maps every live device and, via ARP, its hardware vendor — enough to fingerprint the network and spot anything that shouldn't be there, before a single port is probed.

---

## ⚠️ Risk Highlights

| Risk Level | Example Finding |
|---|---|
| 🔴 Critical | Internet-exposed IoT/camera devices located via GHDB |
| 🟠 Medium | CMS/plugin versions exposed; DNS & mail infrastructure details exposed; multiple live hosts on local network |
| 🟢 Low | Server IP identifiable; WAF product identifiable; contact email discoverable |

> Findings above are *observations from reconnaissance*, not confirmed/exploited vulnerabilities — no exploitation was performed or attempted.

## 🛠️ Recommendations (top 5 of 12)

1. Keep CMS, plugins and DNS software patched and reviewed against current advisories.
2. Minimise information leaked in HTTP headers and DNS records.
3. Secure, VPN-gate, or remove any internet-facing camera/IoT device found exposed.
4. Disable directory indexing on production web servers.
5. Perform regular internal network discovery and investigate any unrecognised device.

*(Full list of 12 recommendations in the submitted report.)*

## 📂 Repository Structure

```
├── README.md              → this file
└── screenshots/           → evidence captured for each tool/module
    ├── whois.png
    ├── whatweb.png
    ├── nslookup.png
    ├── curl.png
    ├── wafw00f.png
    ├── dnsrecon.png
    ├── maltego_email.png
    ├── theharvester_results.png
    ├── zenmap.png
    └── zenmap_topology.pdf
```

> 🔒 **Note on redaction:** in line with the program's guidance not to publish real IP addresses or directly reachable device links on public/social platforms, exact IP addresses, camera device links and full MAC address tables have been kept in my private submitted report and are intentionally omitted or summarised here.

## ⚖️ Disclaimer

This project was carried out strictly for educational purposes as part of the Networkwalks Cybersecurity & Ethical Hacking internship (Batch B082). All activity stayed within the authorized scope described above. Nothing in this repository should be used to test or access any system without explicit written authorization. Unauthorized access to computer systems is illegal in most jurisdictions.

## 👤 Author

**Abubakar Issa Sabalah**
Cybersecurity & Ethical Hacking Intern — Batch B082, Networkwalks
Program: [networkwalks.com](https://networkwalks.com)
