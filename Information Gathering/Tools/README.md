A detailed, hands-on guide to every essential **Information Gathering** tool a beginner needs. Each tool is explained from scratch — what it does, how it works under the hood, installation, commands, flags, and when to use it in a real engagement.

### What You Need
- A machine running **Kali Linux**, **Parrot OS**, or any Debian-based Linux distro
- Basic familiarity with the Linux terminal
- A practice target  — **never test on real targets without permission**)

### Installing All Tools at Once (Kali Linux)
```bash
# Update your system first
sudo apt update && sudo apt upgrade -y

# Install tools available via apt
sudo apt install nmap whois dnsutils theharvester whatweb nikto gobuster netcat-openbsd curl exiftool -y

# Install subfinder (Go-based)
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest

# Install Shodan CLI
pip3 install shodan

# Install SpiderFoot
pip3 install spiderfoot

# Install Recon-ng
git clone https://github.com/lanmaster53/recon-ng.git
cd recon-ng && pip3 install -r REQUIREMENTS

# Maltego — download from https://www.maltego.com/downloads/
```

---

## Tools Covered

| # | Tool | Type | What It Does |
|---|------|------|-------------|
| 1 | **Nmap** | Active | Discovers open ports, services, OS, and versions |
| 2 | **Whois** | Passive | Reveals domain owner, registrar, name servers |
| 3 | **Dig** | Passive/Active | Queries DNS records (A, MX, NS, TXT, etc.) |
| 4 | **theHarvester** | Passive | Gathers emails, subdomains, IPs from public sources |
| 5 | **WhatWeb** | Active | Identifies web technologies, CMS, frameworks |
| 6 | **Nikto** | Active | Scans web servers for misconfigurations and known issues |
| 7 | **Gobuster** | Active | Brute-forces hidden directories, files, and subdomains |
| 8 | **Subfinder** | Passive | Discovers subdomains from passive public sources |
| 9 | **Netcat** | Active | Banner grabbing and raw TCP/UDP connections |
| 10 | **Curl** | Active | Inspects HTTP headers, responses, and endpoints |
| 11 | **Shodan CLI** | Passive | Searches internet-exposed devices and services |
| 12 | **ExifTool** | Passive | Extracts hidden metadata from files and images |
| 13 | **Maltego** | Passive | Visual link analysis and OSINT mapping |
| 14 | **SpiderFoot** | Passive | Automated all-in-one OSINT framework |
| 15 | **Recon-ng** | Passive | Modular web reconnaissance framework |

---
## Legal Disclaimer

> ⚠️ **IMPORTANT — READ THIS**

All tools documented in this guide are to be used **exclusively on systems you own or have received explicit written authorization to test**.

Using these tools against unauthorized systems is **illegal** and may violate:
- 🇺🇸 **USA:** Computer Fraud and Abuse Act (CFAA)
- 🇬🇧 **UK:** Computer Misuse Act 1990
- 🇮🇳 **India:** Information Technology Act 2000
- 🇪🇺 **EU:** Directive 2013/40/EU on Attacks Against Information Systems

**Safe and legal ways to practice:**
- Build your own lab with VMs
- Use platforms like TryHackMe, HackTheBox with authorized rooms
- Participate in **Bug Bounty programs** (HackerOne, Bugcrowd, Intigriti)

---

<div align="center">

**⭐ Star this repo if it helped you on your security journey!**

*Learn ethically. Practice legally. Hack responsibly.*

</div>
