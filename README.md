# 🛡️ Web Application Security & Penetration Testing

> A curated knowledge base and reference guide for web application security concepts, methodologies, tools, and resources — for learners, practitioners, and security researchers.

---

## 📌 Table of Contents

- [About](#about)
- [What is Web Application Security?](#what-is-web-application-security)
- [What is Penetration Testing?](#what-is-penetration-testing)
- [Penetration Testing Phases](#penetration-testing-phases)
- [Common Vulnerabilities](#common-vulnerabilities)
- [Tools](#tools)
- [Resources](#resources)
- [Learning Platforms & Labs](#learning-platforms--labs)
- [Certifications](#certifications)
- [Contributing](#contributing)
- [Disclaimer](#disclaimer)

---

## About

This repository is a comprehensive reference for **web application security** and **penetration testing**. Whether you're a beginner starting your journey in cybersecurity or an experienced practitioner looking for a quick reference, this repo covers foundational concepts, methodologies, tools, and curated external resources.

> ⚠️ **Disclaimer:** All content in this repository is intended strictly for **educational and authorized testing purposes only**. Unauthorized access to systems is illegal. Always obtain proper written permission before conducting any penetration test.

---

## What is Web Application Security?

**Web Application Security** is the practice of protecting websites, web applications, and web services from malicious attacks and vulnerabilities. It encompasses a broad range of practices, techniques, and tools designed to ensure the **confidentiality**, **integrity**, and **availability** (CIA triad) of web-based systems and their data.

### Key Goals
- Protect sensitive user and business data
- Prevent unauthorized access and privilege escalation
- Ensure application availability and resilience
- Comply with regulations (GDPR, PCI-DSS, HIPAA, etc.)

---

## What is Penetration Testing?

**Penetration Testing** (or "pentesting") is an authorized, simulated cyberattack performed on a system, network, or web application to identify exploitable vulnerabilities before malicious actors do.

### Types of Penetration Testing

| Type | Description |
|------|-------------|
| **Black Box** | Tester has no prior knowledge of the target system |
| **White Box** | Tester has full knowledge including source code and architecture |
| **Grey Box** | Tester has partial knowledge (e.g., user-level credentials only) |

### Penetration Testing Standards & Methodologies

- **OWASP Testing Guide (OTG)** — Industry-standard web application testing methodology
- **PTES** — Penetration Testing Execution Standard
- **OSSTMM** — Open Source Security Testing Methodology Manual
- **NIST SP 800-115** — Technical Guide to Information Security Testing and Assessment

---

## Penetration Testing Phases

```
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│  1. PLANNING &   │────▶│  2. RECONNAIS-   │────▶│   3. SCANNING &  │
│  RECONNAISSANCE  │     │    SANCE         │     │   ENUMERATION    │
└──────────────────┘     └──────────────────┘     └──────────────────┘
                                                           │
                                                           ▼
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│   6. REPORTING   │◀────│ 5. POST-EXPLOIT- │◀────│  4. EXPLOITATION │
│                  │     │    ATION         │     │                  │
└──────────────────┘     └──────────────────┘     └──────────────────┘
```

### Phase Breakdown

#### 1. 🗺️ Planning & Reconnaissance
Define scope, rules of engagement, and goals. Gather passive information about the target using OSINT techniques (WHOIS, DNS lookups, Google Dorking, Shodan).

#### 2. 🔍 Scanning & Enumeration
Actively probe the target to identify open ports, services, software versions, and technology stacks. Tools: Nmap, Nikto, WhatWeb, Gobuster.

#### 3. 💥 Exploitation
Attempt to exploit discovered vulnerabilities to gain unauthorized access. This simulates what a real attacker might do. Tools: Burp Suite, SQLMap, Metasploit.

#### 4. 🔑 Post-Exploitation
After gaining access, assess the potential impact — escalate privileges, move laterally, and identify sensitive data exposure.

#### 5. 📄 Reporting
Document findings with clear risk ratings (Critical/High/Medium/Low), evidence (screenshots, payloads), and actionable remediation recommendations.

---

## Common Vulnerabilities

Based on the **OWASP Top 10** (the most critical web application security risks):

| # | Vulnerability | Description |
|---|---------------|-------------|
| 01 | **Broken Access Control** | Users can act outside their intended permissions |
| 02 | **Cryptographic Failures** | Weak or missing encryption of sensitive data |
| 03 | **Injection** | SQLi, NoSQLi, OS Command Injection, LDAP Injection |
| 04 | **Insecure Design** | Missing or ineffective security controls by design |
| 05 | **Security Misconfiguration** | Default credentials, open cloud storage, verbose errors |
| 06 | **Vulnerable & Outdated Components** | Using libraries/frameworks with known CVEs |
| 07 | **Identification & Auth Failures** | Weak passwords, brute-force, session fixation |
| 08 | **Software & Data Integrity Failures** | Insecure CI/CD pipelines, unsigned updates |
| 09 | **Security Logging & Monitoring Failures** | Inability to detect and respond to breaches |
| 10 | **Server-Side Request Forgery (SSRF)** | Server fetches attacker-controlled URLs |

### Other Notable Vulnerabilities

- **Cross-Site Scripting (XSS)** — Reflected, Stored, DOM-based
- **Cross-Site Request Forgery (CSRF)**
- **Insecure Direct Object Reference (IDOR)**
- **XML External Entity (XXE) Injection**
- **Business Logic Flaws**
- **Open Redirects**
- **Clickjacking**
- **HTTP Request Smuggling**

---

## Tools

### 🕵️ Reconnaissance
| Tool | Description |
|------|-------------|
| [Nmap](https://nmap.org/) | Network scanner for open ports and services |
| [Shodan](https://www.shodan.io/) | Search engine for internet-connected devices |
| [theHarvester](https://github.com/laramies/theHarvester) | OSINT tool for emails, subdomains, IPs |
| [Amass](https://github.com/owasp-amass/amass) | In-depth attack surface mapping |
| [Subfinder](https://github.com/projectdiscovery/subfinder) | Subdomain discovery tool |

### 🔬 Scanning & Enumeration
| Tool | Description |
|------|-------------|
| [Nikto](https://github.com/sullo/nikto) | Web server vulnerability scanner |
| [Gobuster](https://github.com/OJ/gobuster) | Directory/file and DNS brute-forcer |
| [Wfuzz](https://github.com/xmendez/wfuzz) | Web application fuzzer |
| [WhatWeb](https://github.com/urbanadventurer/WhatWeb) | Web technology fingerprinter |
| [ffuf](https://github.com/ffuf/ffuf) | Fast web fuzzer written in Go |

### ⚔️ Exploitation & Testing
| Tool | Description |
|------|-------------|
| [Burp Suite](https://portswigger.net/burp) | Industry-standard web app security testing proxy |
| [OWASP ZAP](https://www.zaproxy.org/) | Open-source web application scanner |
| [SQLMap](https://sqlmap.org/) | Automatic SQL injection detection and exploitation |
| [Metasploit](https://www.metasploit.com/) | Exploit development and execution framework |
| [XSStrike](https://github.com/s0md3v/XSStrike) | Advanced XSS detection suite |

### 🔐 Password & Auth Testing
| Tool | Description |
|------|-------------|
| [Hydra](https://github.com/vanhauser-thc/thc-hydra) | Fast network login cracker |
| [Hashcat](https://hashcat.net/hashcat/) | Advanced password recovery tool |
| [John the Ripper](https://www.openwall.com/john/) | Open-source password security auditing tool |

---

## Resources

### 📚 Official Documentation & Standards
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — The most critical web security risks
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/) — Comprehensive web app testing methodology
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/) — Quick developer security references
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework) — Risk-based security framework
- [CWE/SANS Top 25](https://cwe.mitre.org/top25/) — Most dangerous software weaknesses
- [CVE Database](https://cve.mitre.org/) — Common Vulnerabilities and Exposures

### 📖 Books
- *The Web Application Hacker's Handbook* — Stuttard & Pinto
- *Hacking: The Art of Exploitation* — Jon Erickson
- *Real-World Bug Hunting* — Peter Yaworski
- *The Hacker Playbook 3* — Peter Kim
- *Bug Bounty Bootcamp* — Vickie Li

### 🎥 YouTube Channels
- [LiveOverflow](https://www.youtube.com/@LiveOverflow) — Hacking explained visually
- [IppSec](https://www.youtube.com/@ippsec) — HTB machine walkthroughs
- [John Hammond](https://www.youtube.com/@_JohnHammond) — CTFs and malware analysis
- [TCM Security](https://www.youtube.com/@TCMSecurityAcademy) — Ethical hacking courses

---

## Learning Platforms & Labs

| Platform | Description | Free/Paid |
|----------|-------------|-----------|
| [PortSwigger Web Security Academy](https://portswigger.net/web-security) | Free, hands-on web security labs by the makers of Burp Suite | Free |
| [TryHackMe](https://tryhackme.com/) | Beginner-friendly guided hacking rooms | Free/Paid |
| [HackTheBox](https://www.hackthebox.com/) | Real-world penetration testing labs | Free/Paid |
| [OWASP WebGoat](https://owasp.org/www-project-webgoat/) | Deliberately insecure app for learning | Free |
| [DVWA](https://dvwa.co.uk/) | Damn Vulnerable Web Application | Free |
| [PentesterLab](https://pentesterlab.com/) | Web pentesting exercises with badges | Free/Paid |
| [VulnHub](https://www.vulnhub.com/) | Downloadable vulnerable VMs | Free |
| [PicoCTF](https://picoctf.org/) | CTF challenges for beginners | Free |

---

## Certifications

| Certification | Body | Level |
|---------------|------|-------|
| **CompTIA Security+** | CompTIA | Beginner |
| **eJPT** | eLearnSecurity | Beginner |
| **CEH** — Certified Ethical Hacker | EC-Council | Intermediate |
| **OSCP** — Offensive Security Certified Professional | OffSec | Advanced |
| **BSCP** — Burp Suite Certified Practitioner | PortSwigger | Intermediate |
| **GWAPT** — GIAC Web Application Penetration Tester | GIAC | Intermediate |

---

## Contributing

Contributions are welcome! To contribute:

1. Fork this repository
2. Create a new branch (`git checkout -b feature/your-topic`)
3. Make your changes and commit (`git commit -m 'Add: your topic'`)
4. Push to the branch (`git push origin feature/your-topic`)
5. Open a Pull Request

Please ensure all contributions are:
- Accurate and up to date
- Relevant to web application security or pentesting
- Free of tools or content that promote illegal activity

---

## Disclaimer

> This repository is intended **solely for educational purposes** and for use in **authorized security testing environments**. The authors and contributors are not responsible for any misuse of the information provided here.
>
> **Always get explicit written permission** before testing any system or application you do not own. Unauthorized penetration testing is a criminal offense in most jurisdictions.

---

<div align="center">

**⭐ Star this repo if you find it helpful!**

Made with ❤️ for the security community

</div>
