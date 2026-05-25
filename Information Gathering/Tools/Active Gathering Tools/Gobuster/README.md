## 7. Gobuster — Directory & DNS Brute-Forcer

### How It Works
**Gobuster** uses **wordlists** to brute-force hidden directories, files, DNS subdomains, and virtual hostnames. It sends HTTP requests (or DNS queries) for each word in the list and reports which ones return a valid response.

```
Gobuster reads wordlist:
  admin
  login
  backup
  config
  ...
     │
     ├── GET /admin    → 200 OK       ✅ Found!
     ├── GET /login    → 302 Redirect ✅ Found!
     ├── GET /backup   → 403 Forbidden ✅ Found! (still interesting)
     ├── GET /config   → 404 Not Found ❌ Skip
     └── GET /test     → 200 OK       ✅ Found!
```

### Installation
```bash
# Kali (pre-installed)
gobuster version

# Ubuntu / Debian
sudo apt install gobuster -y

# From source (Go)
go install github.com/OJ/gobuster/v3@latest
```

### Wordlists (Essential!)

Gobuster needs wordlists to work. The best source is **SecLists**:

```bash
# Install SecLists
sudo apt install seclists -y
# Location: /usr/share/seclists/

# OR download manually
git clone https://github.com/danielmiessler/SecLists.git /opt/seclists

# Most commonly used wordlists:
/usr/share/seclists/Discovery/Web-Content/common.txt           # 4,700 entries — fast
/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt  # 220k entries
/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt  # Subdomain list
/usr/share/wordlists/dirb/common.txt                          # Dirb common list
```

### Modes of Operation

#### DIR Mode — Directory & File Brute-Forcing

```bash
# Basic directory brute-force
gobuster dir -u http://example.com -w /usr/share/wordlists/dirb/common.txt

# With file extensions
gobuster dir -u http://example.com \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -x php,html,txt,bak,sql,zip

# Faster scan with more threads
gobuster dir -u http://example.com \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -t 50

# Show specific status codes only
gobuster dir -u http://example.com \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -s "200,204,301,302,307,403"

# Include response lengths (helps filter false positives)
gobuster dir -u http://example.com \
  -w /usr/share/wordlists/dirb/common.txt \
  -l

# Save output to file
gobuster dir -u http://example.com \
  -w /usr/share/wordlists/dirb/common.txt \
  -o directories.txt
```

#### DNS Mode — Subdomain Brute-Forcing

```bash
# Basic subdomain enumeration
gobuster dns -d example.com \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt

# Show IP of found subdomains
gobuster dns -d example.com \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -i

# Use custom DNS resolver
gobuster dns -d example.com \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -r 8.8.8.8
```

#### VHOST Mode — Virtual Host Discovery

```bash
# Find virtual hosts on a shared server
gobuster vhost -u http://example.com \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

### Important Flags

| Flag | What It Does | Example |
|------|-------------|---------|
| `-u` | Target URL | `-u http://target.com` |
| `-w` | Wordlist path **(required)** | `-w /path/to/wordlist.txt` |
| `-t` | Number of threads (default 10) | `-t 50` |
| `-x` | File extensions to append | `-x php,html,txt` |
| `-o` | Output file | `-o results.txt` |
| `-s` | Positive status codes | `-s "200,302,403"` |
| `-b` | Negative status codes to skip | `-b "404,500"` |
| `-l` | Show response content length | `-l` |
| `-r` | Follow redirects | `-r` |
| `-k` | Skip TLS certificate verification | `-k` |
| `-c` | Add cookies | `-c "session=abc123"` |
| `-H` | Add custom headers | `-H "Authorization: Bearer token"` |
| `-U` | HTTP basic auth username | `-U admin` |
| `-P` | HTTP basic auth password | `-P password` |
| `-q` | Quiet mode (no banner) | `-q` |
| `-v` | Verbose — show all attempts | `-v` |

### Reading the Output

```
===============================================================
Gobuster v3.4
===============================================================
[+] Url:                     http://example.com
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Status codes:            200,204,301,302,307,401,403,405,500
[+] Threads:                 10
===============================================================
/admin                (Status: 301) [Size: 0] [--> /admin/]    ← Admin redirect
/backup               (Status: 403) [Size: 287]               ← Forbidden but exists!
/config               (Status: 200) [Size: 1024]              ← Config file!
/login                (Status: 200) [Size: 4561]              ← Login page
/uploads              (Status: 301) [Size: 0] [--> /uploads/]
/robots.txt           (Status: 200) [Size: 45]                ← Check this!
```

### Use Cases
- Find **hidden admin panels** (/admin, /administrator, /dashboard)
- Discover **backup files** (.bak, .old, .zip, .sql)
- Find **configuration files** (config.php, .env, settings.py)
- Enumerate **API endpoints** (/api/v1/, /api/users, /api/admin)
- Discover **upload directories** that could allow file access

---
