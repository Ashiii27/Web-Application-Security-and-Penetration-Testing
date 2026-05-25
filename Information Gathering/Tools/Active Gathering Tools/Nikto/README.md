## 6. Nikto — Web Server Scanner

### How It Works
**Nikto** is an open-source web server scanner that performs comprehensive tests against web servers for:
- Dangerous files and scripts
- Outdated server software
- Server misconfigurations
- Default credentials and pages
- HTTP methods that shouldn't be enabled
- Cookie and security header issues

Nikto works by sending HTTP requests to the target and comparing responses against its **database of 6,700+ potentially dangerous files, CGIs, and known vulnerabilities**.

```
Nikto
  │
  ├── Checks server headers         → Version disclosure
  ├── Tests dangerous CGI files     → /cgi-bin/test-cgi
  ├── Checks for default files      → /phpmyadmin/, /admin/
  ├── Tests HTTP methods            → PUT, DELETE enabled?
  ├── Checks for outdated software  → Apache < 2.4.51
  ├── Validates SSL configuration   → Weak ciphers?
  └── Checks security headers       → Missing HSTS, CSP?
```

### Installation
```bash
# Kali (pre-installed)
nikto -Version

# Ubuntu / Debian
sudo apt install nikto -y

# From source
git clone https://github.com/sullo/nikto.git
cd nikto/program && perl nikto.pl -Version
```

### Basic Usage

```bash
# Basic scan
nikto -h http://example.com

# Scan HTTPS target
nikto -h https://example.com

# Scan on a specific port
nikto -h http://example.com -p 8080

# Scan multiple ports
nikto -h http://example.com -p 80,8080,8443
```

### Important Flags

| Flag | What It Does | Example |
|------|-------------|---------|
| `-h` | Target host **(required)** | `-h http://target.com` |
| `-p` | Target port | `-p 8080` |
| `-ssl` | Force SSL mode | `-ssl` |
| `-o` | Output file | `-o results.html` |
| `-Format` | Output format (html, csv, txt, xml) | `-Format html` |
| `-Tuning` | Scan only specific test types | `-Tuning 1234` |
| `-id` | Use HTTP auth (user:pass) | `-id admin:admin` |
| `-useproxy` | Use a proxy (e.g. Burp Suite) | `-useproxy http://127.0.0.1:8080` |
| `-timeout` | Request timeout in seconds | `-timeout 10` |
| `-maxtime` | Max scan time | `-maxtime 30m` |
| `-noslash` | Skip adding trailing slash | `-noslash` |
| `-C all` | Check all CGI directories | `-C all` |

### Tuning Options (Scan Specific Tests)

```bash
# Tuning codes
# 0 — File Upload
# 1 — Interesting Files / Seen in logs
# 2 — Misconfiguration / Default Files
# 3 — Information Disclosure
# 4 — Injection (XSS/Script/HTML)
# 5 — Remote File Retrieval (Inside Web Root)
# 6 — Denial of Service
# 7 — Remote File Retrieval (Server-Wide)
# 8 — Command Execution / Remote Shell
# 9 — SQL Injection
# a — Authentication Bypass
# b — Software Identification
# c — Remote Source Inclusion

# Example: Only scan for misconfigs and info disclosure
nikto -h http://example.com -Tuning 23

# Scan for SQL injection and XSS only
nikto -h http://example.com -Tuning 94
```

### Save Output as HTML Report

```bash
# Save as HTML (best for sharing)
nikto -h http://example.com -o report.html -Format html

# Save as CSV (for importing to spreadsheets)
nikto -h http://example.com -o report.csv -Format csv

# Run through Burp Suite proxy (to capture traffic)
nikto -h http://example.com -useproxy http://127.0.0.1:8080
```

### Reading the Output

```
- Nikto v2.1.6
+ Target IP:     93.184.216.34
+ Target Port:   80
+ Start Time:    2024-01-15 10:30:00

+ Server: Apache/2.4.29 (Ubuntu)           ← Server version (check CVEs)
+ /: Retrieved x-powered-by header: PHP/7.2.1  ← PHP version exposed
+ /robots.txt: Contains 3 entries           ← Check robots.txt for hidden paths
+ /phpmyadmin/: phpMyAdmin directory found  ← DB admin panel exposed!
+ OSVDB-630: /admin/: Administration page   ← Admin panel found
+ /backup/: Backup directory found          ← Sensitive files may be here
+ Cookie PHPSESSID set without HttpOnly     ← XSS risk on session cookie
+ Missing X-Frame-Options header            ← Clickjacking risk
+ Missing Content-Security-Policy header    ← XSS risk
+ Apache/2.4.29 appears outdated            ← Known CVEs may apply
```

### Use Cases
- Quickly **audit a web server** for low-hanging fruit
- Find **exposed admin panels** (/phpmyadmin/, /admin/, /wp-admin/)
- Detect **default files** left by the server or CMS installation
- Identify **misconfigured HTTP methods** (PUT/DELETE/TRACE)
- Find **missing security headers** that leave the app vulnerable
- Detect **outdated software** with known CVEs

---
