
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

## 🔑 Key Findings (summary)

- **WHOIS** — domain registered via GoDaddy/HostGator, privacy-protected registrant, DNSSEC unsigned.
- **WhatWeb** — WordPress 7.0.4 + WP Download Manager 3.3.58 running on Apache; server IP and contact email exposed.
- **Nslookup** — domain resolves to a public IPv4 address.
- **Curl -I** — HTTP headers revealed the active security plugin ("Really Simple Security") and a caching layer.
- **Wafw00f** — site is protected by ModSecurity (SpiderLabs) WAF.
- **DNSrecon** — 8 DNS records enumerated (NS, MX, TXT/SPF, SRV) plus DNS server software version.
- **GHDB / Google dorking** — located multiple internet-exposed IoT/camera interfaces and open directory listings using advanced search operators (exact links withheld from this public repo — see private report).
- **Maltego** — mapped one public contact email and two related URLs from the domain via a transform graph.
- **theHarvester** — harvested several sub-domains and emails for a public target domain from free OSINT sources; all-source mode is limited on a fresh Kali install without paid API keys.
- **Zenmap** — discovered 25 live hosts on my local `/24` subnet with IP/MAC/vendor data, and exported a network topology diagram.

> 🔒 **Note on redaction:** in line with the program's guidance not to publish real IP addresses or directly reachable device links on public/social platforms, exact IP addresses, camera device links and full MAC address tables have been kept in my private submitted report and are intentionally omitted or summarised here.

## ⚠️ Risk Highlights

| Risk Level | Example Finding |
|---|---|
| 🔴 Critical | Internet-exposed IoT/camera devices located via GHDB |
| 🟠 Medium | CMS/plugin versions exposed; DNS & mail infrastructure details exposed; multiple live hosts on local network |
| 🟢 Low | Server IP identifiable; WAF product identifiable; contact email discoverable |

## 🛠️ Recommendations (top 5)

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

## 📜 Scope & Authorization

All testing against `networkwalks.com` was carried out under a signed Letter of Authorization (Ref. NW-LOA-B082-017) issued by Networkwalks for the internship period. My own LAN was tested under owner's consent. `microsoft.com` was queried only through theHarvester's passive, public-source lookups — no direct connection was made to that domain's infrastructure.

## ⚖️ Disclaimer

This project was carried out strictly for educational purposes as part of the Networkwalks Cybersecurity & Ethical Hacking internship (Batch B082). All activity stayed within the authorized scope described above. Nothing in this repository should be used to test or access any system without explicit written authorization. Unauthorized access to computer systems is illegal in most jurisdictions.

## 👤 Author

**Abubakar Issa Sabalah**
Cybersecurity & Ethical Hacking Intern — Batch B082, Networkwalks
Program: [networkwalks.com](https://networkwalks.com)
