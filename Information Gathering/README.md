# 🔍 Information Gathering — Complete Guide
### From Beginner to Advanced

> A complete, structured guide to mastering Information Gathering (Reconnaissance) in cybersecurity and penetration testing — covering concepts, techniques, tools, and real-world workflows.

---

## 📌 Table of Contents

- [What is Information Gathering?](#what-is-information-gathering)
- [Why Information Gathering Matters](#why-information-gathering-matters)
- [Types of Reconnaissance](#types-of-reconnaissance)
- [The Information Gathering Framework](#the-information-gathering-framework)
- [🟢 Beginner Level](#-beginner-level)
  - [Google Dorking](#1-google-dorking)
  - [WHOIS Lookup](#2-whois-lookup)
  - [DNS Enumeration](#3-dns-enumeration)
  - [Email Harvesting](#4-email-harvesting)
  - [Netcraft & Shodan (Basic)](#5-netcraft--shodan-basic)
- [🟡 Intermediate Level](#-intermediate-level)
  - [Subdomain Enumeration](#1-subdomain-enumeration)
  - [Port Scanning & Service Detection](#2-port-scanning--service-detection)
  - [Web Technology Fingerprinting](#3-web-technology-fingerprinting)
  - [SSL/TLS Certificate Inspection](#4-ssltls-certificate-inspection)
  - [Social Engineering Recon](#5-social-engineering-recon)
  - [Metadata Extraction](#6-metadata-extraction)
- [🔴 Advanced Level](#-advanced-level)
  - [Attack Surface Mapping](#1-attack-surface-mapping)
  - [ASN & IP Range Discovery](#2-asn--ip-range-discovery)
  - [Git & Source Code Recon](#3-git--source-code-recon)
  - [Cloud Asset Discovery](#4-cloud-asset-discovery)
  - [Shodan & Censys Deep Dive](#5-shodan--censys-deep-dive)
  - [Passive DNS & Historical Data](#6-passive-dns--historical-data)
  - [OSINT Automation Pipelines](#7-osint-automation-pipelines)
- [Tools Reference](#tools-reference)
- [Real-World Workflow](#real-world-workflow)
- [Reporting & Documentation](#reporting--documentation)
- [Resources & Further Learning](#resources--further-learning)
- [Legal & Ethical Considerations](#legal--ethical-considerations)

---

## What is Information Gathering?

**Information Gathering** (also called **Reconnaissance** or **Recon**) is the first and most critical phase of any penetration test or security assessment. It is the process of **systematically collecting data about a target** — such as a domain, organization, IP range, or person — to understand the attack surface before any active testing begins.

> 💡 **The golden rule:** The more you know about a target, the more effective and focused your testing will be. Skilled attackers spend 70–80% of their time on reconnaissance.

Information gathering answers questions like:
- What domains and subdomains does the target own?
- What IP ranges and cloud infrastructure do they use?
- What software, frameworks, and versions are exposed?
- Who are the employees and what are their email formats?
- Are there any exposed credentials, secrets, or sensitive documents?

---

## Why Information Gathering Matters

```
Poor Recon  ──▶  Random, noisy attacks  ──▶  Easily detected, low success rate
Good Recon  ──▶  Targeted, stealthy attacks  ──▶  Higher impact, lower noise
```

| Without Recon | With Recon |
|---------------|------------|
| Guessing attack vectors | Knowing exactly where to look |
| Triggering IDS/WAF alerts | Staying under the radar |
| Missing exposed assets | Finding every entry point |
| Wasting time on dead ends | Focusing on high-value targets |

---

## Types of Reconnaissance

### Passive Reconnaissance
Collecting information **without directly interacting** with the target. Uses public sources. Leaves **no traces** on the target's systems.

**Examples:** Google Dorking, WHOIS, DNS lookups, Shodan, certificate transparency logs, social media OSINT.

### Active Reconnaissance
**Directly interacting** with the target's systems to gather information. May be **logged or detected** by the target.

**Examples:** Port scanning, web crawling, directory brute-forcing, banner grabbing.

```
┌─────────────────────────────────────────────────────────┐
│                   RECONNAISSANCE                        │
│                                                         │
│   ┌──────────────────────┐  ┌──────────────────────┐   │
│   │   PASSIVE RECON      │  │   ACTIVE RECON       │   │
│   │                      │  │                      │   │
│   │ • Search engines     │  │ • Port scanning      │   │
│   │ • WHOIS / DNS        │  │ • Service probing    │   │
│   │ • Social media       │  │ • Dir brute-forcing  │   │
│   │ • Certificate logs   │  │ • Banner grabbing    │   │
│   │ • Shodan / Censys    │  │ • Web spidering      │   │
│   │ • Job postings       │  │ • DNS zone transfer  │   │
│   │                      │  │                      │   │
│   │  ✅ No target trace  │  │  ⚠️ May be detected  │   │
│   └──────────────────────┘  └──────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## The Information Gathering Framework

A structured approach ensures nothing is missed:

```
DEFINE SCOPE
     │
     ▼
PASSIVE RECON ──────────────────────────────────────────┐
  • Domain & DNS info                                    │
  • IP ranges & ASNs                                     │
  • Employee & org data                                  │
  • Technology stack                                     │
  • Historical & leaked data                             │
     │                                                   │
     ▼                                                   │
ACTIVE RECON                                             │
  • Port & service scanning                              │
  • Web crawling & fuzzing                               │
  • Certificate inspection                               │
  • Cloud asset discovery                                │
     │                                                   │
     ▼                                                   │
ANALYSIS & MAPPING                                       │
  • Build attack surface map                             │
  • Identify high-value targets                          │
  • Prioritize entry points                              │
     │                                                   │
     ▼                                                   │
DOCUMENTATION & REPORTING  ◀──────────────────────────── ┘
```

---

## 🟢 Beginner Level

> Start here if you're new to information gathering. These techniques use publicly available data and simple tools.

---

### 1. Google Dorking

**Google Dorking** (or Google Hacking) uses advanced search operators to find sensitive information indexed by Google — exposed files, login pages, cameras, configuration files, and more.

#### Essential Google Dork Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `site:` | Limit results to a specific domain | `site:example.com` |
| `inurl:` | Search for keywords in the URL | `inurl:admin` |
| `intitle:` | Search for keywords in page title | `intitle:"index of"` |
| `intext:` | Search for keywords in page body | `intext:"password"` |
| `filetype:` | Find specific file types | `filetype:pdf` |
| `cache:` | View Google's cached version | `cache:example.com` |
| `ext:` | Alternative to filetype | `ext:sql` |
| `"..."` | Exact phrase match | `"admin password"` |
| `-` | Exclude a keyword | `site:example.com -www` |

#### Useful Dork Combinations

```
# Find exposed admin panels
site:target.com inurl:admin OR inurl:login OR inurl:dashboard

# Find exposed config/backup files
site:target.com ext:env OR ext:bak OR ext:config OR ext:sql

# Find exposed documents
site:target.com filetype:pdf OR filetype:xlsx OR filetype:docx

# Find directory listings
intitle:"index of" site:target.com

# Find exposed credentials in files
site:target.com intext:"password" filetype:log

# Find subdomains
site:*.target.com -www

# Find cameras / login pages
inurl:"/view/index.shtml" intitle:"Live View"
```

#### 📦 Resources
- [Google Hacking Database (GHDB)](https://www.exploit-db.com/google-hacking-database) — Huge collection of dorks
- [DorkSearch](https://dorksearch.com/) — Dork builder and search tool

---

### 2. WHOIS Lookup

**WHOIS** is a protocol that queries databases containing registration information about domain names and IP addresses — revealing the owner, registrar, registration dates, name servers, and sometimes contact details.

#### What WHOIS Reveals

```
Domain Name:     EXAMPLE.COM
Registrar:       GoDaddy.com, LLC
Created:         1995-08-14
Updated:         2023-08-13
Expiry:          2024-08-13
Name Servers:    NS1.EXAMPLE.COM
                 NS2.EXAMPLE.COM
Registrant:      Example Corp
Email:           admin@example.com
Country:         US
```

#### WHOIS Commands

```bash
# Basic WHOIS lookup
whois example.com

# IP address WHOIS
whois 93.184.216.34

# Find all domains registered to an email
whois -h whois.domaintools.com "email@example.com"

# Find registrar info only
whois example.com | grep -i registrar
```

#### Online WHOIS Tools
- [who.is](https://who.is/)
- [DomainTools](https://www.domaintools.com/)
- [ICANN WHOIS](https://lookup.icann.org/)
- [ViewDNS](https://viewdns.info/)

> 💡 **Note:** GDPR/WHOIS privacy services often redact personal info. Use OSINT techniques to uncover the real registrant.

---

### 3. DNS Enumeration

**DNS (Domain Name System)** is a goldmine of information. DNS enumeration reveals subdomains, mail servers, IP addresses, zone configurations, and more.

#### Key DNS Record Types

| Record | Purpose | Example |
|--------|---------|---------|
| `A` | Maps domain → IPv4 | `example.com → 93.184.216.34` |
| `AAAA` | Maps domain → IPv6 | `example.com → 2606:2800::1` |
| `MX` | Mail exchange servers | `mail.example.com` |
| `NS` | Name servers | `ns1.example.com` |
| `TXT` | SPF, DKIM, verification | `v=spf1 include:...` |
| `CNAME` | Alias to another domain | `www → example.com` |
| `SOA` | Start of Authority | Admin, serial, refresh info |
| `PTR` | Reverse DNS (IP → domain) | `34.216.184.93.in-addr.arpa` |

#### DNS Enumeration Commands

```bash
# Basic DNS lookup
nslookup example.com
dig example.com

# Get all record types
dig example.com ANY

# Query specific record types
dig example.com MX
dig example.com TXT
dig example.com NS

# Reverse DNS lookup (PTR)
dig -x 93.184.216.34

# DNS Zone Transfer attempt (may be restricted)
dig axfr @ns1.example.com example.com

# Use a specific DNS server
dig @8.8.8.8 example.com

# Get short answer
dig +short example.com
```

#### Tools
- **nslookup** — Built-in Windows/Linux DNS query tool
- **dig** — Flexible DNS lookup utility (Linux)
- **host** — Simple DNS lookup tool
- [DNSdumpster](https://dnsdumpster.com/) — Online DNS reconnaissance tool
- [MXToolbox](https://mxtoolbox.com/) — DNS & mail server checks

---

### 4. Email Harvesting

Finding **employee email addresses** reveals naming conventions (e.g., `firstname.lastname@company.com`), key personnel, and potential phishing targets.

#### Manual Methods

```
# LinkedIn: Search for employees at the company
# Hunter.io: Find emails by domain
# GitHub: Search commits for email addresses
# Job postings: Reveal tech stack and sometimes contacts
```

#### Email Harvesting Tools

```bash
# theHarvester — email, subdomain, and name gathering
theHarvester -d example.com -b google,linkedin,bing,github

# theHarvester from specific source
theHarvester -d example.com -b hunter

# Find emails on GitHub
# Search: "example.com" in:email
```

#### Verify Email Addresses

```bash
# Check if email exists (MX record method)
dig example.com MX

# Online verification tools
# https://hunter.io/email-verifier
# https://verify-email.org/
```

#### Email Naming Pattern Examples

| Format | Example |
|--------|---------|
| `first.last@domain.com` | john.doe@company.com |
| `flast@domain.com` | jdoe@company.com |
| `firstl@domain.com` | johnd@company.com |
| `first@domain.com` | john@company.com |

---

### 5. Netcraft & Shodan (Basic)

#### Netcraft
[Netcraft](https://sitereport.netcraft.com/) provides detailed reports on a website's:
- Hosting history and IP address history
- Web server software and version
- SSL/TLS certificate info
- Site uptime history

```
Visit: https://sitereport.netcraft.com/?url=example.com
```

#### Shodan — The Hacker's Search Engine

**Shodan** indexes internet-connected devices and their banners. It reveals:
- Open ports and services
- Software versions
- Geographic location
- SSL certificates
- Known vulnerabilities (CVEs)

```
# Basic Shodan searches (via web interface)
hostname:example.com
org:"Target Organization"
ip:93.184.216.34
port:3306 country:US         # MySQL servers in the US
product:nginx version:1.14   # Specific nginx version
```

---

## 🟡 Intermediate Level

> You're comfortable with the basics. Now go deeper with automated tools and structured enumeration techniques.

---

### 1. Subdomain Enumeration

Subdomains often host **staging environments, admin panels, APIs, and forgotten legacy apps** that are far less hardened than the main site.

#### Methods of Subdomain Discovery

**1. Brute-force with Wordlists**

```bash
# Gobuster — directory and DNS brute-forcer
gobuster dns -d example.com -w /usr/share/wordlists/subdomains.txt -t 50

# ffuf — fast fuzzer
ffuf -w /usr/share/wordlists/subdomains.txt -u https://FUZZ.example.com -v

# Sublist3r
sublist3r -d example.com -t 50 -o subdomains.txt
```

**2. Certificate Transparency Logs**

Certificate transparency logs publicly record every SSL certificate ever issued — a goldmine for subdomains.

```bash
# crt.sh — certificate transparency search
curl -s "https://crt.sh/?q=%.example.com&output=json" | jq '.[].name_value' | sort -u

# Via browser
# https://crt.sh/?q=%.example.com
```

**3. Automated Multi-source Tools**

```bash
# Subfinder — fastest passive subdomain discovery
subfinder -d example.com -o subdomains.txt

# Amass — comprehensive ASM tool
amass enum -passive -d example.com -o amass_output.txt
amass enum -active -d example.com -brute -o amass_active.txt

# Assetfinder
assetfinder --subs-only example.com

# Combine and sort unique results
cat subdomains.txt amass_output.txt | sort -u > all_subs.txt
```

**4. DNS Brute-force with Massdns**

```bash
massdns -r /path/to/resolvers.txt -t A -o S -w results.txt subdomains.txt
```

#### Probing Live Subdomains

```bash
# httpx — check which subdomains are live
cat all_subs.txt | httpx -status-code -title -tech-detect -o live_subs.txt

# httprobe — simple HTTP/HTTPS prober
cat all_subs.txt | httprobe -c 50 > live_hosts.txt
```

---

### 2. Port Scanning & Service Detection

**Port scanning** maps which services are running and what versions, helping identify potential attack vectors.

#### Nmap — The Essential Scanner

```bash
# Quick scan — top 1000 ports
nmap example.com

# Full TCP port scan
nmap -p- example.com

# Service version + OS detection
nmap -sV -O example.com

# Aggressive scan (version + OS + scripts + traceroute)
nmap -A example.com

# Stealth SYN scan (requires root)
nmap -sS example.com

# UDP scan (slower, important for DNS, SNMP)
nmap -sU example.com

# Script scan — use NSE scripts
nmap --script=http-title,http-headers example.com
nmap --script=vuln example.com              # Vulnerability scan
nmap --script=ssl-enum-ciphers example.com  # SSL cipher check

# Output to file (all formats)
nmap -oA scan_results example.com

# Scan multiple targets
nmap -iL targets.txt -oA output
```

#### Masscan — Ultra-fast Port Scanner

```bash
# Scan entire internet range (use responsibly and with permission!)
masscan 192.168.1.0/24 -p 80,443,8080 --rate=1000

# Full port scan of a single target
masscan example.com -p 0-65535 --rate=10000
```

#### Interpreting Common Ports

| Port | Service | Notes |
|------|---------|-------|
| 21 | FTP | Check for anonymous login |
| 22 | SSH | Check for weak credentials, old versions |
| 23 | Telnet | Insecure, plaintext |
| 25 | SMTP | Email server, relay check |
| 53 | DNS | Zone transfer attempts |
| 80/443 | HTTP/HTTPS | Web app testing |
| 3306 | MySQL | Check for remote access |
| 3389 | RDP | Remote desktop, brute-force risk |
| 6379 | Redis | Often unauthenticated |
| 8080/8443 | Alt HTTP/S | Dev servers, proxies |
| 27017 | MongoDB | Often unauthenticated |

---

### 3. Web Technology Fingerprinting

Identifying the **tech stack** of a web application reveals the frameworks, CMS, server software, and libraries in use — which can then be checked for known CVEs.

```bash
# WhatWeb — technology fingerprinting
whatweb https://example.com
whatweb -v https://example.com  # Verbose output
whatweb -a 3 https://example.com  # Aggression level 3

# Wappalyzer CLI
npx wappalyzer https://example.com

# httpx with technology detection
echo "https://example.com" | httpx -tech-detect

# curl — manual header inspection
curl -I https://example.com
curl -v https://example.com 2>&1 | grep -i "x-powered-by\|server:\|x-generator"
```

#### What to Look For

- **Server header:** `Apache/2.4.29`, `nginx/1.14.0`, `Microsoft-IIS/10.0`
- **X-Powered-By:** `PHP/7.2.1`, `ASP.NET`
- **Set-Cookie:** Reveals session framework (e.g., `PHPSESSID`, `JSESSIONID`, `laravel_session`)
- **CMS hints:** WordPress, Joomla, Drupal paths (`/wp-admin/`, `/administrator/`)

---

### 4. SSL/TLS Certificate Inspection

SSL certificates contain **Subject Alternative Names (SANs)** — a list of all domains the cert is valid for. This often reveals internal hostnames and additional subdomains.

```bash
# View certificate via openssl
echo | openssl s_client -connect example.com:443 2>/dev/null | openssl x509 -noout -text

# Extract SANs only
echo | openssl s_client -connect example.com:443 2>/dev/null | \
  openssl x509 -noout -ext subjectAltName

# Check certificate expiry
echo | openssl s_client -connect example.com:443 2>/dev/null | \
  openssl x509 -noout -dates

# sslyze — comprehensive SSL/TLS analysis
sslyze example.com

# testssl.sh — check for weak ciphers, BEAST, POODLE, Heartbleed
testssl.sh example.com
```

#### Online Tools
- [SSL Labs](https://www.ssllabs.com/ssltest/) — Comprehensive SSL grading
- [crt.sh](https://crt.sh/) — Certificate transparency search

---

### 5. Social Engineering Recon

**People are often the weakest link.** Gathering information about employees, their roles, and their online presence enables social engineering attacks and targeted phishing.

#### LinkedIn OSINT

```
Useful searches:
• "site:linkedin.com/in/ 'Target Company'"
• Filter by: Current company, job title, location
• Look for: IT staff, executives, HR, helpdesk

What to collect:
• Full names → email format guessing
• Job titles → access levels and responsibilities
• Skills → technology stack clues
• Connections → org chart reconstruction
```

#### Job Posting Analysis

Job postings are goldmines — they accidentally reveal:
- Internal software and tools in use
- Cloud providers and frameworks
- Security maturity gaps ("must know X" = they use X)
- Contact names and emails

```
Search:
• LinkedIn Jobs
• Indeed, Glassdoor
• The company's careers page
• BuiltWith, Stackshare (for tech stack)
```

#### Tools
- [Maltego](https://www.maltego.com/) — Graphical OSINT and link analysis
- [SpiderFoot](https://www.spiderfoot.net/) — Automated OSINT framework
- [Sherlock](https://github.com/sherlock-project/sherlock) — Username hunting across 300+ sites

---

### 6. Metadata Extraction

Documents (PDFs, Word files, images) published by organizations **embed hidden metadata** — author names, usernames, software versions, GPS coordinates, network paths, and more.

```bash
# exiftool — universal metadata extractor
exiftool document.pdf
exiftool image.jpg
exiftool -r /path/to/folder/  # Recursive

# Extract specific fields
exiftool -Author -Creator -Producer document.pdf

# Strings — find readable text in binary files
strings document.pdf | grep -i "user\|author\|path\|server"

# metagoofil — automated document metadata harvester
metagoofil -d example.com -t pdf,doc,xls,ppt -o output/
```

#### What Metadata Can Reveal

| File Type | Potential Metadata |
|-----------|-------------------|
| PDF | Author name, creating software, modification dates |
| Word/Excel | Author, company, revision history, template path |
| JPEG/PNG | GPS coordinates, camera model, timestamps |
| MP3 | Artist, software, album |
| Any file | Internal network paths, usernames, hostnames |

---

## 🔴 Advanced Level

> For experienced practitioners building comprehensive attack surface maps and running automated OSINT pipelines.

---

### 1. Attack Surface Mapping

**Attack Surface Mapping** is the process of identifying every possible entry point into an organization — not just the main website, but all domains, IPs, cloud assets, APIs, and third-party integrations.

#### Components of an Attack Surface

```
                        ┌─────────────────────┐
                        │    ORGANIZATION     │
                        └──────────┬──────────┘
                                   │
        ┌──────────────────────────┼──────────────────────────┐
        │                          │                          │
   ┌────▼─────┐            ┌───────▼──────┐           ┌──────▼────┐
   │  DOMAINS │            │  CLOUD INFRA │           │   PEOPLE  │
   │          │            │              │           │           │
   │ apex     │            │ AWS S3/EC2   │           │ employees │
   │ subs     │            │ GCP / Azure  │           │ emails    │
   │ expired  │            │ Firebase     │           │ creds     │
   └──────────┘            └──────────────┘           └───────────┘
        │                                                    │
   ┌────▼─────┐            ┌──────────────┐           ┌──────▼────┐
   │   APIS   │            │   THIRD-PARTY │           │   CODE    │
   │          │            │              │           │           │
   │ REST     │            │ CDN          │           │ GitHub    │
   │ GraphQL  │            │ SaaS tools   │           │ GitLab    │
   │ SOAP     │            │ Partners     │           │ leaked    │
   └──────────┘            └──────────────┘           └───────────┘
```

#### ASM Workflow

```bash
# Step 1: Start with the apex domain
echo "example.com" > scope.txt

# Step 2: Passive subdomain discovery
subfinder -dL scope.txt -o subs_passive.txt
amass enum -passive -df scope.txt -o subs_amass.txt

# Step 3: Brute-force subdomains
gobuster dns -d example.com -w subdomains-top1million-5000.txt -o subs_brute.txt

# Step 4: Resolve and probe live hosts
cat subs_*.txt | sort -u | httpx -status-code -title -ip -tech-detect -o live_hosts.json

# Step 5: Port scan live hosts
cat live_hosts.json | jq -r '.url' | sed 's|https://||;s|http://||' | nmap -iL - -p- --open -oA full_scan

# Step 6: Screenshot all live URLs
cat live_hosts.txt | gowitness file -f - -P screenshots/
```

---

### 2. ASN & IP Range Discovery

An **Autonomous System Number (ASN)** identifies a network managed by a single organization. Finding a company's ASN reveals **all their IP ranges** — not just those associated with their main domain.

```bash
# Find ASN for an organization
whois -h whois.cymru.com " -v 93.184.216.34"
curl -s "https://api.bgpview.io/search?query_term=Target+Company" | jq

# Find all IP ranges for an ASN
whois -h whois.radb.net -- '-i origin AS15169' | grep -E "^route"

# BGP toolkit
bgp.he.net  # Hurricane Electric BGP Toolkit (web)

# Shodan ASN search
shodan search "org:Target Company" --fields ip_str,port,org --separator ,

# nmap scan of entire ASN range (authorized only!)
nmap -sn 93.184.216.0/24 -oG alive_hosts.txt
grep "Up" alive_hosts.txt | awk '{print $2}' > live_ips.txt
```

#### Online Tools
- [BGP.he.net](https://bgp.he.net/) — BGP and ASN lookup
- [BGPView](https://bgpview.io/) — ASN, IP, and prefix API
- [ARIN](https://search.arin.net/) — IP registration for North America
- [RIPEstat](https://stat.ripe.net/) — IP geolocation and routing data

---

### 3. Git & Source Code Recon

Developers often **accidentally commit secrets** to public repositories — API keys, database credentials, private keys, internal URLs, and more.

#### GitHub Recon

```bash
# Search GitHub for sensitive info (web)
"example.com" password
"example.com" secret
"example.com" api_key
org:ExampleCorp filename:.env
org:ExampleCorp extension:pem

# GitHub Dorking patterns
filename:.env DB_PASSWORD
filename:config.php password
extension:sql password
"private_key" "example.com"
```

#### Automated Git Secret Scanning

```bash
# truffleHog — scans git history for secrets
trufflehog git https://github.com/example/repo
trufflehog github --org=ExampleCorp

# gitleaks — secret detection in repos
gitleaks detect --source /path/to/repo
gitleaks detect --repo-url https://github.com/example/repo

# gitrob — GitHub org reconnaissance
gitrob analyze examplecorp

# git-dumper — dump an exposed .git directory
git-dumper https://example.com/.git /output/dir

# Check for exposed .git on web servers
curl -s https://example.com/.git/HEAD
curl -s https://example.com/.git/config
```

#### What to Look For

```
Secrets:             DB credentials, API keys, JWT secrets, private keys
Config files:        .env, config.yml, application.properties, settings.py
Internal URLs:       Internal APIs, staging servers, admin endpoints
Historical commits:  Deleted files can still be in git history
GitHub Actions:      CI/CD credentials, deploy keys
Docker files:        Base images, exposed ports, hardcoded secrets
```

---

### 4. Cloud Asset Discovery

Organizations increasingly expose assets via **S3 buckets, Azure Blobs, GCP storage, Firebase**, and other cloud services — often misconfigured as public.

#### AWS S3 Bucket Discovery

```bash
# Common bucket naming patterns to check
{company}-backup
{company}-dev
{company}-staging
{company}-assets
{company}-data
{company}-logs

# Check if a bucket is public
curl -s https://{bucket-name}.s3.amazonaws.com/
aws s3 ls s3://bucket-name --no-sign-request

# Automated S3 bucket enumeration
bucket-finder wordlist.txt --region us-east-1 --log buckets.log

# s3scanner
s3scanner scan --buckets-file buckets.txt

# lazys3
ruby lazys3.rb example
```

#### GCP, Azure, Firebase

```bash
# GCP Storage
curl https://storage.googleapis.com/{bucket-name}/

# Azure Blob
curl https://{account}.blob.core.windows.net/{container}/

# Firebase
curl https://{project-id}.firebaseio.com/.json  # Returns data if public

# CloudEnum — multi-cloud enumeration
python3 cloud_enum.py -k example -k examplecorp
```

---

### 5. Shodan & Censys Deep Dive

#### Advanced Shodan Queries

```
# Web searches
hostname:example.com
org:"Example Corp"
ssl:"example.com"
http.title:"Example Login"
http.html:"example.com"

# Vulnerability hunting
vuln:CVE-2021-44228          # Log4Shell
product:OpenSSH version:7.4  # Old SSH

# Service-specific
port:27017 -authentication   # Unauthenticated MongoDB
port:6379 -authentication    # Unauthenticated Redis
port:9200 product:Elasticsearch  # Elasticsearch
port:5432 product:PostgreSQL

# Geographic
org:"Example Corp" country:US city:"New York"

# Device types
device:webcam
device:router
device:printer

# Combine filters
hostname:example.com port:22 product:OpenSSH
```

#### Shodan CLI

```bash
# Install
pip install shodan

# Configure API key
shodan init YOUR_API_KEY

# Search
shodan search 'org:"Example Corp"'
shodan search 'hostname:example.com'

# Host info
shodan host 93.184.216.34

# Count results
shodan count 'org:"Example Corp"'

# Download results
shodan download results.json.gz 'org:"Example Corp"'
shodan parse results.json.gz --fields ip_str,port,transport
```

#### Censys

```bash
# Censys CLI
pip install censys

# Search hosts
censys search 'parsed.names: example.com' --index-type hosts

# IPv4 hosts
censys view 93.184.216.34 --index-type hosts

# Search certificates
censys search 'parsed.names: example.com' --index-type certificates
```

---

### 6. Passive DNS & Historical Data

**Passive DNS** records historical DNS resolutions — revealing old IPs, subdomains that no longer resolve, and infrastructure changes over time.

```bash
# SecurityTrails API
curl -s "https://api.securitytrails.com/v1/domain/example.com/subdomains" \
  -H "APIKEY: YOUR_KEY" | jq

# Historical DNS
curl -s "https://api.securitytrails.com/v1/history/example.com/dns/a" \
  -H "APIKEY: YOUR_KEY" | jq

# Wayback Machine — find old URLs
curl -s "http://web.archive.org/cdx/search/cdx?url=*.example.com/*&output=text&fl=original&collapse=urlkey"
```

#### Tools for Historical Analysis

| Tool | What It Reveals |
|------|----------------|
| [SecurityTrails](https://securitytrails.com/) | Historical DNS, subdomains, IPs |
| [Wayback Machine](https://web.archive.org/) | Old pages, leaked paths, old JS files |
| [VirusTotal](https://www.virustotal.com/) | Subdomains, resolutions, related files |
| [PassiveTotal (RiskIQ)](https://community.riskiq.com/) | Passive DNS, WHOIS history |
| [DNSlytics](https://dnslytics.com/) | Reverse IP, DNS records |

---

### 7. OSINT Automation Pipelines

At scale, manual reconnaissance is impractical. Build automated pipelines that chain tools together.

#### Sample Recon Pipeline (Bash)

```bash
#!/bin/bash
TARGET=$1
OUTPUT="recon_$TARGET"
mkdir -p $OUTPUT/{subs,ports,screenshots,secrets}

echo "[*] Starting recon on $TARGET"

# 1. Passive subdomain discovery
echo "[*] Gathering subdomains..."
subfinder -d $TARGET -silent -o $OUTPUT/subs/subfinder.txt
amass enum -passive -d $TARGET -o $OUTPUT/subs/amass.txt
curl -s "https://crt.sh/?q=%.$TARGET&output=json" | jq -r '.[].name_value' | \
  sed 's/\*\.//g' | sort -u > $OUTPUT/subs/crt.txt

# 2. Combine unique subdomains
cat $OUTPUT/subs/*.txt | sort -u > $OUTPUT/subs/all_subs.txt
echo "[+] Found $(wc -l < $OUTPUT/subs/all_subs.txt) unique subdomains"

# 3. Probe live hosts
echo "[*] Probing live hosts..."
cat $OUTPUT/subs/all_subs.txt | httpx -silent -status-code -title \
  -tech-detect -o $OUTPUT/live_hosts.txt

# 4. Screenshot live hosts
echo "[*] Taking screenshots..."
gowitness file -f $OUTPUT/live_hosts.txt -P $OUTPUT/screenshots/ --no-http

# 5. Port scan
echo "[*] Port scanning..."
cat $OUTPUT/subs/all_subs.txt | naabu -p 80,443,8080,8443,3000,5000 \
  -silent -o $OUTPUT/ports/open_ports.txt

echo "[+] Recon complete! Results in $OUTPUT/"
```

#### ReconFTW — Full Automated Framework

```bash
# Install
git clone https://github.com/six2dez/reconftw.git
cd reconftw && ./install.sh

# Run full recon
./reconftw.sh -d example.com -a -o output/

# Run passive only
./reconftw.sh -d example.com --passive -o output/
```

---

## Tools Reference

### All-in-One Cheatsheet

| Category | Tool | Install | Key Command |
|----------|------|---------|-------------|
| Subdomain | Subfinder | `go install` | `subfinder -d target.com` |
| Subdomain | Amass | `go install` | `amass enum -d target.com` |
| Subdomain | Assetfinder | `go install` | `assetfinder target.com` |
| DNS | Dig | built-in | `dig target.com ANY` |
| DNS | MassDNS | `git clone` | `massdns -r resolvers.txt` |
| Port Scan | Nmap | `apt install` | `nmap -sV -p- target.com` |
| Port Scan | Masscan | `apt install` | `masscan target.com -p 0-65535` |
| Port Scan | Naabu | `go install` | `naabu -host target.com` |
| Web | Httpx | `go install` | `httpx -u target.com -tech-detect` |
| Web | WhatWeb | `apt install` | `whatweb target.com` |
| Web | Gobuster | `apt install` | `gobuster dns -d target.com -w list.txt` |
| Fuzzing | Ffuf | `go install` | `ffuf -u https://FUZZ.target.com -w list.txt` |
| Screenshot | Gowitness | `go install` | `gowitness single https://target.com` |
| OSINT | theHarvester | `apt install` | `theHarvester -d target.com -b all` |
| OSINT | Maltego | Desktop app | GUI-based link analysis |
| OSINT | SpiderFoot | `pip install` | `spiderfoot -s target.com` |
| Secrets | TruffleHog | `pip install` | `trufflehog git https://github.com/...` |
| Secrets | Gitleaks | `go install` | `gitleaks detect --source repo/` |
| Metadata | ExifTool | `apt install` | `exiftool file.pdf` |
| Cloud | CloudEnum | `git clone` | `python3 cloud_enum.py -k company` |
| Shodan | Shodan CLI | `pip install` | `shodan search 'org:Target'` |

---

## Real-World Workflow

Here's how a professional pentester approaches information gathering for a real engagement:

```
DAY 1: Passive Recon
├── WHOIS + DNS records (apex domain and variants)
├── Certificate transparency (crt.sh, censys)
├── Google Dorking (sensitive files, subdomains, login pages)
├── Shodan/Censys (exposed services, IPs)
├── LinkedIn OSINT (employees, email format, tech stack)
├── GitHub search (credentials, API keys, internal URLs)
└── Job postings (technology clues)

DAY 2: Asset Discovery
├── Run subfinder, amass, assetfinder
├── Brute-force subdomains with Gobuster + ffuf
├── Probe live hosts with httpx
├── Screenshot all live URLs with gowitness
└── Build comprehensive subdomain inventory

DAY 3: Active Enumeration
├── Port scan all live hosts with nmap/naabu
├── Fingerprint technologies on all live hosts
├── Check for exposed .git directories
├── Cloud asset enumeration (S3, GCP, Azure)
├── Metadata extraction from public documents
└── Passive DNS history review

DAY 4: Analysis & Mapping
├── Correlate all gathered data
├── Identify high-value targets (admin panels, APIs, old apps)
├── Map full attack surface
├── Prioritize findings by risk
└── Begin reporting draft
```

---

## Reporting & Documentation

Good documentation of your information gathering is critical — for yourself and your clients.

### What to Document

```markdown
## Target: example.com
**Date:** 2024-01-15
**Scope:** *.example.com, 93.184.216.0/24

### Subdomains Discovered
| Subdomain | IP | Status | Technology |
|-----------|-----|--------|------------|
| admin.example.com | 93.184.216.10 | 200 | WordPress 5.9 |
| api.example.com | 93.184.216.11 | 200 | Node.js |
| staging.example.com | 93.184.216.12 | 200 | Laravel |

### Open Ports
| IP | Port | Service | Version |
|----|------|---------|---------|
| 93.184.216.10 | 22 | SSH | OpenSSH 7.4 |
| 93.184.216.10 | 80 | HTTP | Apache 2.4 |

### Notable Findings
- Exposed .git directory at https://staging.example.com/.git
- S3 bucket "example-backups" publicly readable
- Employee emails follow format: first.last@example.com
```

### Recommended Tools for Reporting
- **Obsidian / Notion** — Note-taking and linking
- **CherryTree** — Hierarchical note-taking for pentesters
- **Dradis** — Collaborative reporting platform
- **PlexTrac** — Professional pentest reporting

---

## Resources & Further Learning

### 📚 Books
- *OSINT Techniques* — Michael Bazzell (updated annually, the OSINT bible)
- *The Art of Invisibility* — Kevin Mitnick
- *Open Source Intelligence Techniques* — Michael Bazzell
- *Penetration Testing* — Georgia Weidman

### 🌐 Online Platforms
- [TryHackMe — Rooms](https://tryhackme.com/) — "OSINT", "Passive Reconnaissance", "Active Reconnaissance"
- [HackTheBox](https://www.hackthebox.com/) — Active OSINT challenges
- [PortSwigger Web Security Academy](https://portswigger.net/web-security) — Free web security labs

### 📺 YouTube
- [David Bombal](https://www.youtube.com/@davidbombal) — Networking and hacking
- [TCM Security](https://www.youtube.com/@TCMSecurityAcademy) — Practical ethical hacking courses
- [NahamSec](https://www.youtube.com/@NahamSec) — Bug bounty and recon
- [zseano](https://www.youtube.com/@zseano) — Bug bounty methodology

### 🔗 Online Tools
- [Shodan](https://shodan.io)
- [Censys](https://censys.io)
- [crt.sh](https://crt.sh)
- [DNSdumpster](https://dnsdumpster.com)
- [SecurityTrails](https://securitytrails.com)
- [VirusTotal](https://virustotal.com)
- [Hunter.io](https://hunter.io)
- [IntelTechniques](https://inteltechniques.com/tools/) — OSINT tools by Michael Bazzell

### 📋 Wordlists
- [SecLists](https://github.com/danielmiessler/SecLists) — The ultimate wordlist collection
- [Assetnote Wordlists](https://wordlists.assetnote.io/) — Modern subdomain and path wordlists
- [DNS Wordlists](https://github.com/blechschmidt/massdns/tree/master/lists) — For massdns

---

## Legal & Ethical Considerations

> ⚠️ **THIS IS CRITICAL — READ BEFORE YOU BEGIN**

### Always Get Written Authorization

**Unauthorized reconnaissance is illegal in most jurisdictions**, even if you never touch the target system directly. Always obtain written permission before conducting any form of active or semi-active information gathering.

### What's Generally Allowed (Passive, Public Data)
- ✅ Google searching
- ✅ WHOIS lookups
- ✅ Searching public GitHub repos
- ✅ Viewing public LinkedIn profiles
- ✅ Searching Shodan/Censys for indexed data

### What Requires Authorization
- ⚠️ Port scanning target systems
- ⚠️ Accessing discovered but non-public assets
- ⚠️ Brute-forcing subdomains (sends DNS queries to resolvers)
- ⚠️ Accessing misconfigured S3 buckets / databases
- ⚠️ Attempting DNS zone transfers

### Relevant Laws
- **United States:** Computer Fraud and Abuse Act (CFAA)
- **United Kingdom:** Computer Misuse Act 1990
- **European Union:** Directive on Attacks Against Information Systems
- **India:** Information Technology Act 2000

### Bug Bounty Programs
If you're practicing without a specific client, **Bug Bounty programs** provide legal authorization to test specific scopes. Platforms:
- [HackerOne](https://hackerone.com/)
- [Bugcrowd](https://www.bugcrowd.com/)
- [Intigriti](https://www.intigriti.com/)
- [YesWeHack](https://www.yeswehack.com/)

---

<div align="center">

**⭐ Star this repo if it helped you level up your recon game!**

*Information is the most powerful weapon in a hacker's arsenal. Use it ethically.*

</div>
