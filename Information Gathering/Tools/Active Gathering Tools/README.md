# 🎯 Active Reconnaissance Tools — Complete Guide
### How They Work · How to Use Them · Flags · Use Cases · Real Examples

> Active reconnaissance involves **directly interacting** with the target system to gather information. Unlike passive recon, active recon sends packets, queries, and requests **directly to the target** — which means it can be detected. This guide covers every essential active recon tool from beginner to advanced level.

---

## ⚠️ Important Warning

Active reconnaissance **directly communicates with the target**. This means:
- It **can be logged** by the target's firewall, IDS, or SIEM
- It **may be illegal** without written authorization
- It **can trigger alerts** and get your IP blocked

**Always have written permission before performing active reconnaissance.**



## What is Active Reconnaissance?

**Active Reconnaissance** is the phase where you directly interact with the target's systems to gather information. You are sending real packets, making DNS queries, and probing services — the target's infrastructure **can detect and log** this activity.

```
┌─────────────────────────────────────────────────────────────────┐
│                   ACTIVE RECONNAISSANCE                         │
│                                                                 │
│   You ──── TCP packets ────────────────────────▶ Target        │
│   You ──── DNS queries ────────────────────────▶ Target DNS    │
│   You ──── HTTP requests ──────────────────────▶ Target Web    │
│   You ──── Service probes ─────────────────────▶ Target Ports  │
│                                                                 │
│   ⚠️  Target CAN see your IP and log your activity             │
└─────────────────────────────────────────────────────────────────┘
```

### What Active Recon Discovers
- **Open ports** and running services
- **Software names and versions** (for CVE lookups)
- **Hidden directories and files** on web servers
- **Subdomains** and virtual hosts
- **Web technologies** (CMS, frameworks, libraries)
- **DNS records** and zone data
- **SSL/TLS configuration** weaknesses
- **WAF presence** and type
- **SMB shares**, network file systems

---

## Active vs Passive Recon

```
┌──────────────────────────────────┬──────────────────────────────────┐
│        PASSIVE RECON             │         ACTIVE RECON             │
├──────────────────────────────────┼──────────────────────────────────┤
│ No direct contact with target    │ Direct contact with target       │
│ Uses public data sources         │ Sends real packets/requests      │
│ Cannot be detected by target     │ Can be logged and detected       │
│ Slower, less complete            │ Faster, more complete            │
│ WHOIS, Shodan, crt.sh           │ Nmap, Gobuster, Nikto, ffuf      │
│ Always legal (public data)       │ Requires authorization           │
└──────────────────────────────────┴──────────────────────────────────┘
```

---

## Tools Covered

| # | Tool | Category | Purpose |
|---|------|----------|---------|
| 1 | **Nmap** | Port Scanning | Port, service, OS, and version detection |
| 2 | **Gobuster** | Web Enumeration | Directory, file, DNS, and vhost brute-forcing |
| 3 | **Nikto** | Web Scanning | Web server misconfigurations and known issues |
| 4 | **ffuf** | Fuzzing | Fast web content and parameter fuzzing |
| 5 | **WhatWeb** | Fingerprinting | Web technology and framework detection |
| 6 | **Netcat** | Probing | Banner grabbing and manual port probing |
| 7 | **Curl** | HTTP Probing | Manual HTTP request crafting and inspection |
| 8 | **Dirb** | Web Enumeration | Directory brute-forcing with pattern support |
| 9 | **Enum4linux** | SMB Enumeration | Windows/Samba share and user enumeration |
| 10 | **DNSRecon** | DNS Enumeration | Comprehensive DNS record and zone analysis |
| 11 | **WPScan** | CMS Scanning | WordPress vulnerability and user enumeration |
| 12 | **SSLScan** | SSL Analysis | SSL/TLS cipher, cert, and protocol analysis |
| 13 | **Wafw00f** | WAF Detection | Identifies web application firewalls |
| 14 | **Httprobe** | Host Probing | Identifies live HTTP/HTTPS hosts from a list |
| 15 | **Naabu** | Port Scanning | Fast SYN/CONNECT port scanner |
| 16 | **httpx** | HTTP Toolkit | Multi-function HTTP probing and fingerprinting |
| 17 | **Eyewitness** | Screenshots | Visual mapping of web services |
| 18 | **Masscan** | Port Scanning | Internet-scale ultra-fast port scanner |

---
