## 4. theHarvester — Email & Subdomain OSINT

### How It Works
**theHarvester** is a passive OSINT tool that scrapes multiple public data sources simultaneously — search engines, certificate logs, DNS datasets, GitHub, LinkedIn, and more — to collect:
- Email addresses
- Subdomains
- IP addresses
- Employee names
- Open ports (via Shodan/Censys integration)

```
theHarvester
     │
     ├── Google ──────────▶ emails, subdomains
     ├── Bing ────────────▶ emails, subdomains
     ├── LinkedIn ────────▶ employee names
     ├── Hunter.io ───────▶ emails
     ├── Shodan ──────────▶ IPs, open ports
     ├── crt.sh ──────────▶ SSL cert subdomains
     └── GitHub ──────────▶ emails, code mentions
          │
          ▼
     Aggregated Results
     (deduplicated & organized)
```

### Installation
```bash
# Kali (pre-installed)
theHarvester --help

# Manual install
pip3 install theHarvester
# OR
git clone https://github.com/laramies/theHarvester.git
cd theHarvester && pip3 install -r requirements/base.txt
```

### Basic Usage

```bash
# Basic search using Google
theHarvester -d example.com -b google

# Search multiple sources
theHarvester -d example.com -b google,bing,linkedin

# Search ALL available sources
theHarvester -d example.com -b all

# Limit the number of results
theHarvester -d example.com -b google -l 200

# Save results to HTML report
theHarvester -d example.com -b all -f report.html

# Save results to XML
theHarvester -d example.com -b all -f results.xml
```

### Important Flags

| Flag | What It Does | Example |
|------|-------------|---------|
| `-d` | Target domain **(required)** | `-d example.com` |
| `-b` | Data source to use **(required)** | `-b google,bing` |
| `-l` | Limit number of search results | `-l 500` |
| `-f` | Save output to file (html/xml/json) | `-f output.html` |
| `-v` | Verify discovered hosts via DNS | `-v` |
| `-s` | Start result from this number | `-s 50` |
| `-p` | Use proxies (from proxies.yaml) | `-p` |

### Available Data Sources

```bash
# Check all available sources
theHarvester -h | grep "Supported"

# Key sources and what they find:
# google        → emails, subdomains, URLs
# bing          → emails, subdomains
# linkedin      → employee names
# hunter        → verified emails (API key needed)
# shodan        → IPs, open ports, banners (API key needed)
# censys        → IPs, open ports (API key needed)
# crtsh         → subdomains from SSL certificates
# github-code   → emails from GitHub commits
# dnsdumpster   → subdomains, DNS info
# rapiddns      → subdomains
# sublist3r     → subdomains from multiple sources
```

### Configuring API Keys

```bash
# Edit the API keys config file
nano /path/to/theHarvester/api-keys.yaml

# Add your API keys:
# shodan:
#   key: YOUR_SHODAN_KEY
# hunter:
#   key: YOUR_HUNTER_KEY
```

### Reading the Output

```
[*] Emails found: 4
------------------
admin@example.com
john.doe@example.com
support@example.com
noreply@example.com

[*] Hosts found: 12
-------------------
www.example.com:93.184.216.34
mail.example.com:93.184.216.50
dev.example.com:93.184.216.55
api.example.com:93.184.216.60
```

### Use Cases
- Build a **complete email list** of a target organization
- Discover **subdomains passively** without touching the target
- Guess **email naming conventions** (first.last, f.last, etc.)
- Gather **employee names** from LinkedIn for social engineering research
- Find **IP addresses** linked to a domain

---
